# 01 · Entorno de laboratorio — ThinkPad P14s Gen 2 / i7-1185G7 / 48 GB

## 0. Realidad de tu hardware (léelo antes de comprar nada)

| Recurso | Valor | Implicación técnica |
|---|---|---|
| CPU i7-1185G7 | 4C/8T, VT-x, VT-d, sin vPro | **Máximo 4 vCPU útiles** en total para VMs. Nada de Kali dentro de Kali. |
| RAM 48 GB | ~10-12 GB los consume Windows 11 | **Presupuesto real de VMs: 28-32 GB.** Es tu mayor ventaja. |
| GPU T500 4 GB | ~2048 CUDA cores, sin soporte Hashcat estável | **Hashcat en modo CPU: ~15-35 kH/s en MD5, NTLM ~30-60 kH/s.** Suficiente para CTF/HTB/contraseñas de laboratorio. Insuficiente para campañas de migración de hashes reales. No compres GPU para esto. |
| NVMe 954 GB, ~113 GB usados | ~840 GB libres | Espacio no es el límite. El límite es **RAND** (lectura aleatoria 4K de la VM). |
| Windows 11 64-bit | | Híbrido Hyper-V + WSL2 + VMware/VirtualBox. Hay que elegir arquitectura. |

**Conclusión estratégica:** tu equipo es un *analyst/IR* y un *pentest* machine excelente, no un *malware reversing/cracking* machine. Aprovéchalo para: AD/red/web/blue-team/cloud/SIEM — que es donde está el mercado. Para reversing y cracking, la T500 sí alcanza para malware de Windows en x86/x64 (es CPU-bound, no GPU-bound). Para cracking masivo, usa servicios cloud (hashcat en Hetzner/AWS con GPU) — te explico cómo montarlo en la Fase 3.

---

## 1. Arquitectura recomendada: "Perfil A" (el que debes usar)

```
┌───────────────────────────────────────────────────────────────────────┐
│ HOST WINDOWS 11  ·  ~11 GB RAM en uso                                │
│                                                                       │
│  ┌──────────────┐  ┌──────────────┐  ┌─────────────────────────────┐ │
│  │ VMware Work. │  │  WSL2 Ubuntu │  │  Herramientas nativas Win11 │ │
│  │  Station Pro │  │  (8 GB máx.) │  │  Burp Suite · WinDbg · x64dbg│ │
│  │   (GRATIS)   │  │  Docker      │  │  Ghidra · Sysinternals      │ │
│  └──────┬───────┘  │  (labs web)  │  │  Wireshark · Autopsy       │ │
│         │          └──────────────┘  └─────────────────────────────┘ │
│  ┌──────┴────────────────────┐                                       │
│  │ Kali (attack box)  8 GB / 4 vCPU   VMnet8 NAT + VMnet1 Host-only│ │
│  │ Win11 DC01         4 GB / 2 vCPU   ─┐                            │
│  │ Server 2022 DC02   4 GB / 2 vCPU   ─┼─ VMnet1 192.168.100.0/24   │
│  │ Server 2022 CA01   4 GB / 2 vCPU   ─┘   (aislado de Internet)    │
│  │ VulnHub/HTB box    2-4 GB (1 a la vez)                           │
│  └───────────────────────────────────────                            │
│  Total VMs: 22-26 GB  +  11 GB host  =  ~35 GB   ✔ dentro de 48 GB   │
└───────────────────────────────────────────────────────────────────────┘
```

### Por qué VMware Workstation Pro y no VirtualBox
1. **Desde mayo de 2024 VMware Workstation Pro es 100 % gratuito**, también para uso comercial (Broadcom lo liberó tras la adquisición de VMware). Funcionalidad completa: snapshots, clones, folders compartidos, VIX/API, mejor rendimiento con VMDK.
2. Los VMDK con streaming y TRIM/discard funcionan mucho mejor en NVMe que los VDI de VirtualBox.
3. Snapshots en caliente más fiables.
4. VirtualBox queda como alternativa ligera si quieres una tercera VM sin tocar la instalación principal.

### ⚠️ Decisión crítica: Hyper-V activado o no
Windows 11 con **WSL2 y Docker Desktop necesita "Plataforma de máquina virtual"**, que es la misma capa de hipervisor que Hyper-V. Activado eso, VMware Workstation pierde rendimiento (hasta 10-20 % en E/S).

| Si tu prioridad es… | Configuración |
|---|---|
| **Pentesting de red/AD con muchas VMs** | Desactiva Hyper-V y Docker Desktop; usa **Docker dentro de la VM Kali** solo si lo necesitas. VMware rinde al 100 %. |
| **Blue team / SIEM / análisis con muchos contenedores** | Activa Hyper-V/WMP y usa **Hyper-V Manager** o VMware con backend Hyper-V. Acepta el ~15 % de pérdida. |
| **Equilibrio (recomendado) — Perfil A** | Activa WSL2 (lo necesitas), deja VMware con **"usar hypervisor de Windows" = sí**, y limita a **1 VM pesada a la vez**. El coste es aceptable si no lanzas 4 VMs simultáneas. |

Comprobación del estado actual:
```powershell
# ¿Hyper-V / VMP activados?
Get-WindowsOptionalFeature -Online | Where-Object FeatureName -match 'Hyper-V|VirtualMachinePlatform|Microsoft-Windows-Subsystem-Linux|Containers-Disposable'
```
Si quieres desactivarlos y volver al máximo rendimiento de VM:
```powershell
Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux -NoRestart
Disable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform -NoRestart
# PERO: esto rompe Docker Desktop y WSL2. Decide antes.
```

---

## 2. Instalación en Windows 11 (orden recomendado)

### 2.1 Base del sistema (hazlo primero)
```powershell
# Ejecute en PowerShell como Administrador, en este orden:
# 1) Actualiza Windows hasta "Estás al día" (Windows Update) y reinicia

# 2) Instala Windows Terminal + PowerShell 7 + Git + VS Code + Python + 7-Zip + Notepad++
winget install --id Microsoft.WindowsTerminal
winget install --id Microsoft.PowerShell
winget install --id Git.Git
winget install --id Microsoft.VisualStudioCode
winget install --id Python.Python.3.12
winget install --id 7zip.7zip
winget install --id Notepad++.Notepad++

# 3) Java 21 (necesario para Burp Suite, Ghidra y OWASP ZAP)
winget install --id EclipseAdoptium.Temurin.21.JRE

# 4) Nmap (incluye Npcap) — en Windows SOLO para escaneo básico
winget install --id Nmap.Nmap
```
> Si un `winget install` devuelve "No se encontró el paquete", ve a la web oficial del fabricante. `winget search <nombre>` te ayuda.

### 2.2 Hipervisor
```powershell
# VMware Workstation Pro (gratuito desde 2024)
winget install --id VMware.WorkstationPro
# Si falla: descarga desde https://support.broadcom.com/ (antes techdocs.broadcom.com)

# Alternativa / complemento ligero
winget install --id Oracle.VirtualBox
```

### 2.3 WSL2 + Docker (para labs web en contenedores)
```powershell
wsl --install -d Ubuntu-22.04
wsl --set-default-version 2
wsl --install -d kali-linux     # segundo distro, útil como terminal rápida

winget install --id Docker.DockerDesktop
# Docker Desktop → Settings → Resources → limit WSL integration a Ubuntu-22.04
```

**`.wslconfig`** en `C:\Users\PC\.wslconfig` — **crítico** para que Docker/WSL no te devoren la RAM:
```ini
[wsl2]
memory=8GB
processors=4
swap=4GB
localhostForwarding=true
# autoMemoryReclaim=gradual   # requiere .wslconfig con WSL 2.0+
```
Aplica con: `wsl --shutdown`.

### 2.4 Docker: límites de memoria
Docker Desktop → Settings → Resources → Advanced → **Memory limit: 6-8 GB**, CPU limit 4. Nunca dejes Docker por defecto (por defecto usa hasta la mitad de la RAM del host).

### 2.5 Herramientas de seguridad en Windows (instala por grupos)

**Red / análisis de tráfico**
```powershell
winget install --id WiresharkFoundation.Wireshark
# Ten en cuenta que el driver Npcap pide permiso: acéptalo (es un driver de captura legítimo)
```
Extra: descarga **NetCat for Windows** (`https://gitlab.com/badbits/nc.exe` o `https://github.com/int0x33/nc.exe`) y **tshark**, que viene incluido con Wireshark.

**Sysinternals Suite** (descarga el .zip del enlace oficial y descomprime en `C:\Tools\Sysinternals`):
`https://learn.microsoft.com/sysinternals/downloads` — `Procmon`, `Autoruns`, `Process Explorer`, `TCPView`, `Sigcheck`, `Streams`, `Regshot`, `AccessChk`, `Psexec`, `PsExec` (para practice), `WinDbg`.

**Malware / reversing**
- **x64dbg** → `https://x64dbg.com/` (descarga el `.zip` de la release *snapshot*)
- **Ghidra** (NSA, gratis) → `https://github.com/NationalSecurityAgency/ghidra/releases` (necesita JDK 21 → ya instalado)
- **Detect It Easy (DiE)** → `https://github.com/horsicq/Detect-It-Easy/releases`
- **PE-bear** → `https://github.com/hasherezade/pe-bear/releases`
- **FLOSS (flare-floss)** → `https://github.com/mandiant/flare-floss/releases` (extrae strings ofuscados)
- **capa (Mandiant)** → `https://github.com/mandiant/capa/releases` + `https://github.com/mandiant/capa-rules`
- **PEStudio** → `https://winitor.com/` (gratis, excelente para el triaje rápido de ejecutables)
- **Any.Run sandbox** (cuenta gratuita, 60 min/semana) → `https://any.run/`
- **Triage sandbox** (gratis, sin install) → `https://tria.ge/`
- **CAPE Sandbox público** → `https://capesandbox.com/`

**Web**
```powershell
# Burp Suite Community (gratis, JVM)
winget install --id PortSwigger.BurpSuite.Community
# Si falla: https://portswigger.net/burp/communitydownload
```
- **OWASP ZAP** (segundo proxy, ideal para automatizar) → `https://www.zaproxy.org/download/`
- **Nuclei** → `winget install --id ProjectDiscovery.Nuclei` (o binario en GitHub)

**Forense**
- **Autopsy** → `https://www.autopsy.com/download/` (Windows, ~1 GB)
- **KAPE** (triaje) → `https://github.com/KAPE/KAPE`
- **Cyber Triage** (gratis, portable) → `https://cybertriage.com/`
- **Plaso / log2timeline** → `https://github.com/log2timeline/plaso` (mejor en Docker/WSL: `docker run -v ... ghcr.io/log2timeline/plaso`)
- **Timesketch** → `https://github.com/google/timesketch` (Docker)

**Blue team / SIEM local**
- **Wazuh** (SIEM/XDR open source, la mejor opción para tu RAM) → instalador en `https://wazuh.com` o Docker en Ubuntu Server.
- **Elastic Security** (gratis, ~500 MB/día de ingesta) → `https://www.elastic.co/downloads`. Ten en cuenta que Kibana + Elasticsearch se comen ~6-8 GB: **no lo corras junto a 4 VMs a la vez**.
- **Splunk BOTS datasets** (SIEM datasets gratuitos para práctica) → `https://www.splunk.com/en_us/training`. No necesitas cuenta Splunk Enterprise.
- **Security Onion** → EVITAR en este portátil (necesita 32 GB+). Úsalo solo en otro equipo o en cloud.

**OSINT**
- **Maltego Community** → `https://www.maltego.com/downloads`
- **SpiderFoot** → `https://www.spiderfoot.net/download/`
- **theHarvester** → `https://github.com/laramies/theHarvester` (se instala en Kali)

**Extras de Windows**
```powershell
winget install --id VideoLAN.VLC          # para estudiar con mp4
# Advanced IP Scanner: https://www.advanced-ip-scanner.com/download/
# Everything (búsqueda instantánea de ficheros): https://www.voidtools.com/
# Rufus (para grabar ISOs en USB): https://rufus.ie/
```

### 2.6 Exclusión de Defender (SOLO en tu máquina de laboratorio)
Windows Defender bloquea herramientas de pentest legítimas. **No lo desactives globalmente**; crea un usuario de laboratorio y aplica exclusiones **por ruta**, solo a las carpetas de herramientas:
```powershell
# Ejecutar en PowerShell como Administrador
Add-MpPreference -ExclusionPath "C:\Tools"
Add-MpPreference -ExclusionPath "C:\VMs"
Add-MpPreference -ExclusionProcess "burpsuite.exe"
Add-MpPreference -ExclusionProcess "x64dbg.exe"
Add-MpPreference -ExclusionProcess "SysinternalsSuite.exe"
Add-MpPreference -ExclusionProcess "autoruns.exe"
Add-MpPreference -ExclusionProcess "procmon.exe"
# Ver exclusiones:
Get-MpPreference | Select-Object -ExpandProperty ExclusionPath
```
Documenta esta decisión en tu cuaderno: en un entorno real **jamás** se hace esto; se usan máquinas de análisis aisladas o permit-listing de hashes. Conocer la diferencia es parte de tu formación.

### 2.7 Extensiones de navegador (perfil de laboratorio)
Crea un **perfil de navegador nuevo y aislado** (no uses tu perfil personal):
- **FoxyProxy** o **Proxy SwitchyOmega** (para Burp/ZAP manual)
- **Wappalyzer** (fingerprinting)
- **Burp Suite Extension** (para el Chromium interno de Burp)
- **Cookie Editor** (manipulación de cookies/sesiones)
- **Disable JavaScript** (para bloquear JS en pruebas XSS)
- Desactiva el arranque con extensiones de VPN en ese perfil.

### 2.8 No sincronices las VMs
**Nunca** guardes VMs dentro de carpetas sincronizadas por OneDrive/Dropbox. OneDrive deja los `.vmdk` sparse en estado "online-only", los descarga bajo demanda y corrompe los discos de VM. Las VMs van siempre a `C:\VMs\`.

---

## 3. Configuración óptima de la VM Kali (ya la tienes instalada)

Si la VM Kali ya existe, verifica/aplica esta configuración. Si no la tienes, crea una nueva con estos parámetros.

### 3.1 Parámetros de VM (VMware Workstation Pro)
| Parámetro | Valor | Razón |
|---|---|---|
| Guest OS | **Debian 12 x64** (o "Other Linux 64-bit 3.x") | VMware no reconoce bien "Kali Linux"; declarar *Debian* da mejor rendimiento y tiempos de arranque. |
| vRAM | **8192 MB** | 8 GB es el óptimo: 4 GB para el SO + 2 GB para Burp/navegador + 2 GB de margen. No pongas 12 GB: perjudica al host. |
| vCPU | **1 socket / 4 cores** | Coincide con tus 4C/8T reales. Virtualizar 8 vCPU sobre 4 físicos genera contención. |
| Virtualize Intel VT-x/EPT | **ON** | Necesario si más adelante montas otro hipervisor dentro de Kali (emulador de Android, o un DC anidado). |
| Disco | **120 GB preasignado, NVMe (0:0)** | "Preallocated" evita fragmentación; tu NVMe tiene espacio de sobra. "Allow to grow": OFF. |
| Trim/Discard | **ON** | Reclamación de bloques en NVMe. Evita que la VM crezca a 400 GB con archivos borrados. |
| Network adapter 1 | **VMnet8 (NAT)** → Internet | Descarga de herramientas, `apt`, CTF online. |
| Network adapter 2 | **VMnet1 (Host-only)** → `192.168.100.50/24` | Laboratorio aislado con tus otras VMs. |
| 3D acceleration | **OFF** | La T500 no aporta nada dentro de la VM y genera inestabilidad. |
| USB controller | **3.1** | Necesario para pasar un dispositivo malicioso al sandbox. |
| Carpetas compartidas | **OFF** en la VM principal; crea una VM "sucia" separada con folders ON | Ver sección de OPSEC. |

### 3.2 Configurar VMnet1 en Host-only con subnet propia
En VMware: **Edit → Virtual Network Editor → Change Settings (Admin)**.
- **VMnet1**: tipo *Host-only*, subnet `192.168.100.0`, máscara `255.255.255.0`. **Desmarca** "Use DHCP Server" si no quieres que asigne IPs (lo configuramos estático en la VM).
- **VMnet8**: deja NAT `192.168.x.0/24` (por defecto `192.168.186.0` o `192.168.203.0`).
- Añade `192.168.100.1/24` al Windows como IP estática si quieres que el host sea enrutador/lab host: 
```powershell
New-NetIPAddress -InterfaceAlias "VMware Network Adapter VMnet1" -IPAddress 192.168.100.1 -PrefixLength 24
```

### 3.3 Preparar Kali recién/desactualizada
```bash
# Sistema
sudo apt update && sudo apt full-upgrade -y && sudo apt autoremove -y
sudo apt clean && sudo apt autoclean

# Herramientas VMware en Linux (open-vm-tools, gratis)
sudo apt install -y open-vm-tools open-vm-tools-desktop
sudo systemctl enable --now open-vm-tools
# Si no hay clipboard/drag&drop: sudo usermod -aG vboxsf $USER  (solo si usas VirtualBox)
```

```bash
# Metapaquetes de Kali: instala SOLO lo que necesitas (ahorra 40+ GB)
sudo apt install -y kali-linux-default
sudo apt install -y \
  kali-tools-web kali-tools-information-gathering \
  kali-tools-exploitation kali-tools-passwords \
  kali-tools-privilege-escalation kali-tools-sniffing-spoofing \
  kali-tools-crypto-stego kali-tools-forensics \
  kali-tools-reverse-engineering kali-tools-reporting
# Los que NO debes instalar por ahora: kali-tools-social-engineering, kali-tools-hardware,
# kali-tools-wireless (salvo que tengas adaptador WiFi compatible con modo monitor),
# kali-linux-full (son +15 GB que no vas a usar)
```

```bash
# wordlists: descomprimir rockyou (solo la primera vez; tarda ~1 min)
sudo gzip -dk /usr/share/wordlists/rockyou.txt.gz
ls -lh /usr/share/wordlists/rockyou.txt
# seclists
sudo apt install -y seclists
# (alternativa: git clone https://github.com/danielmiessler/SecLists -C /opt/SecLists)
```

```bash
# Python / pipx (Kali bloquea pip del sistema: usa pipx SIEMPRE)
sudo apt install -y python3-pipx pipx
export PATH="$HOME/.local/bin:$PATH"
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc

pipx install impacket
pipx install netexec                 # reemplazo moderno de CrackMapExec (CrackMapExec está deprecado)
pipx install bloodhound-python
pipx install pwntools
pipx install croniter
pipx install dnsrecon
pipx install Ngrok
```
> `NetExec` (`https://github.com/Pennyw0rth/NetExec`) es **el estándar en 2024-2025**:CrackMapExec está abandonado. Aprende NetExec primero.

```bash
# Go (para instalar herramientas moderno)
sudo apt install -y golang-go git
# Ejemplos:
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
go install -v github.com/jaeles-project/jaeles@latest
# Herramientas/binarios útiles (descarga manual de releases):
#   chisel   → https://github.com/jpillora/chisel
#   ligolo-ng→ https://github.com/nicocha30/ligolo-ng
#   pwncat   → https://github.com/pwntester/pwncat
#   Sliver   → https://github.com/sliverhq/sliver  (C2, compila o descarga release)
#   Sysinternals (Windows binaries) → https://github.com/Sysinternals  o kali: sudo apt install sysinternals
```

```bash
# Kali como cliente OpenVPN/Tor (proxychains)
sudo apt install -y tor proxychains4 openvpn
# Configura /etc/proxychains4.conf (SOCKS5 127.0.0.1 9050)
```

### 3.4 Configuración manual de red dentro de Kali
```bash
# Ver interfaces
ip -br a
# Configurar la interfaz host-only estática (ajusta el nombre: ens33/ens37/ens34)
sudo ip link set ens33 up
sudo ip addr add 192.168.100.50/24 dev ens33
# Persistir (Netplan)
sudo tee /etc/netplan/01-lab.yaml > /dev/null <<'EOF'
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: no
      addresses: [192.168.100.50/24]
EOF
sudo netplan apply
```
Añade `192.168.100.1` a `/etc/hosts`:
```bash
echo "192.168.100.1  lab-host" | sudo tee -a /etc/hosts
```

### 3.5 Atajos y QoL en Kali
```bash
# 1) Terminal: perfil de shell
cat >> ~/.bashrc <<'EOF'
alias ll='ls -la'
alias ..='cd ..'
alias pwncat='pwncat -l 443 --exec /bin/bash'
export HISTSIZE=10000
export HISTFILESIZE=20000
# Cuidado: esto no protege de cerrar sesión por error en un SSH de laboratorio
alias rmv='rm -rfv'
EOF
```

```bash
# 2) Vim/nano: instálalos si no están
sudo apt install -y vim git curl wget
```

```bash
# 3) Kali no trae git por defecto en algunos casos
sudo apt install -y git
git config --global user.name "TU NOMBRE"
git config --global user.email "tu@email.com"
```

### 3.6 Kali como "dirty box" (VM de análisis/malware)
Crea una **segunda VM Kali** de 4 GB / 2 vCPU con:
- Carpetas compartidas **OFF**
- Clipboard bidireccional **OFF** (`vmware-toolbox-cmd config clipboard.enabled false`)
- Red: **solo VMnet8 NAT** (o desconectada cuando analices muestras)
- **Nunca** la uses para pentest que requiera exfiltrar datos reales
- Es tu sandbox de reversing/forense.

### 3.7 Snapshots: política
```bash
# Política de snapshots
#  1) Snapshot "BASE-LIMPIO": recién instalada, herramientas listas, sin labs.
#  2) Antes de CADA lab → snapshot  "lab-<nombre>-<AAAAMMDD>"
#  3) Máximo 3 snapshots: si tienes más, borra los antiguos (los VMDK se fragmentan)
#  4) Al terminar un lab → REVERT a BASE-LIMPIO
```
Desde la CLI de VMware:
```powershell
vmrun -T ws snapshot "C:\VMs\kali\kali.vmx" "lab-htb-htb01-20260927"
vmrun -T ws revertToSnapshot "C:\VMs\kali\kali.vmx" "BASE-LIMPIO"
vmrun -T ws listSnapshots "C:\VMs\kali\kali.vmx"
```

### 3.8 Backups del laboratorio
```bash
# En Kali, backup de tu configuración y notas al host
sudo apt install -y restic
export RESTIC_REPOSITORY="sftp:user@192.168.100.1:/restic-repo"  # o s3/b2
restic init
restic backup ~/.config ~/.local/bin ~/labs-notes ~/scripts
restic forget --prune --keep-daily 7 --keep-weekly 5 --keep-monthly 12
```
- Guarda el **VMDK de BASE-LIMPIO comprimido** en un disco externo (VBoxManage/Vmware: exportar a OVF).
- Tus **notas y writeups** en Git (privado) o en un repo con `git` — el activo más valioso a largo plazo.

---

## 4. Docker en el host: labs web en contenedores

Con Docker Desktop (WSL2), lanza apps vulnerables bajo demanda (1 o 2 a la vez para no saturar):

```powershell
# Ejecute en PowerShell (Docker Desktop abierto)
docker run --rm -it -p 80:80 vulnerables/web-dvwa
docker run --rm -d -p 8080:8080 bkimminich/juice-shop
docker run --rm -d -p 8080:8080 webgoat/webgoat
docker run --rm -d -p 3000:3000 dindv/dvwa
docker run --rm -d -p 3000:3000 vulnerable/metasploitable3   # ojo: hay que bajar la imagen
# En Kali (NAT) estas IPs no se ven. Publica en 0.0.0.0:
docker run -d -p 0.0.0.0:80:80 vulnerables/web-dvwa
```
**Acceso desde Kali VM (VMnet8 NAT):** el gateway NAT de VMware suele ser `192.168.x.1`. Comprueba:
```powershell
ipconfig | Select-String "VMnet8"
```
Y desde Kali, apunta a esa IP del host, p. ej. `http://192.168.203.1`. O más limpio: usa la IP host-only `192.168.100.1`.

**Cuidado con Docker en la VM Kali:** evita `docker` dentro de Kali (requiere virtualización anidada y puede fallar). Usa el Docker del **host** y accede desde Kali por la red host-only.

---

## 5. Máquinas de Active Directory (Fase 2+)

### Opción A: Windows Server Evaluation + manual (gratis, 100 % real)
1. Descarga la ISO **Windows Server 2022 Evaluation** (180 días gratis) → `https://www.microsoft.com/evalcenter/`
2. Instala 3 VMs:
   - **DC01**: 4 GB / 2 vCPU → `promote to domain controller`, bosque nuevo `lab.local`, DNS + ADDS + **CA Enterprise** (necesario para labs de ADCS)
   - **CA01** (opcional): 4 GB / 2 vCPU → segunda CA para ADCS abuse
   - **WIN11-CLIENT**: 4 GB / 2 vCPU → Windows 11 Enterprise Evaluation (`https://www.microsoft.com/evalcenter/`)
3. Red: todas en **VMnet1 host-only** (`192.168.100.0/24`), **desconectadas de Internet**
4. Datos de ejemplo: crea usuarios, **AGEPASS (password en claro en description)**, `BackupAdmin` con grupo `Backup Operators` (historia clásica de DCSync), SPN en `MSSQLSvc` (para Kerberoasting), una cuenta de servicio con `SeImpersonatePrivilege` (para GodPotato / JuicyPotato).

### Opción B: GOAD (Game of AD) — la más rápida y gratis
**GOAD** (Orange Cyberdefense, open source) levanta un AD vulnerable completo con Vagrant/Ansible:
```bash
git clone https://github.com/Orange-Cyberdefense/GOAD
cd GOAD
# Requisitos: VirtualBox o VMware + Vagrant + Ansible
vagrant up
# Variantes: GOAD (Windows+Linux), GOAD-Light (3 VMs, 8 GB RAM), GOAD-TheEmpire, GOAD-RastaLabs
```
- **GOAD-Light** consume ~**8-10 GB de RAM** → **cabe en tu equipo**. Empieza aquí.
- **AD-Miner** (del mismo equipo, gratis) te audita el AD: `https://github.com/Orange-Cyberdefense/AD-Miner`
- Nota: Vagrant sobre VMware/VirtualBox necesita el plugin correcto; verifica la guía del repo.

### Opción C: Snap Labs / Methodius (Azure) — solo si quieres cloud
Microsoft public labs de AD en Azure (gratis con suscripción Azure for Students / $200 de crédito) — úsalos en Fase 3.

---

## 6. Seguridad del entorno de estudio (OPSEC básico)

1. **Autorización primero.** Solo prueba sistemas propios o con autorización **escrita y con alcance definido**. Guarda un documento de "Reglas de Combate" (scope, horas, datos prohibidos) por engagement/lab.
2. **Nunca escanees IPs públicas desde tu IP doméstica** "a ver qué encuentras". Es un riesgo legal serio (en España, art. 264-A de la LSSI; en EE. UU., CFAA y similares). Si haces reconocimiento activo, hazlo desde una VPN de pago o Tor con salida de datacenter, y aun así solo sobre infraestructura **autorizada** (tus propios dominios, VM de pago, programas de bug bounty con scope definido).
3. **Snapshots antes que nada, y red aislada antes que desactivar el antivirus.** Cualquier muestra maliciosa se analiza en la "dirty box" offline.
4. **Cuentas burner** para registros: `33mail.com`, `guerrillamail.com`, aliases de correo.
5. **Perfil de navegador y usuario de Windows separados** para todo lo que toque seguridad.
6. **Activa BitLocker** en el disco del host si el portátil viaja.
7. **Nunca subas muestras de malware a Drive/Dropbox/OneDrive**: el cliente las escanea y las sube. Usa `bazaar.abuse.ch` y VirusTotal (subiendo solo muestras no identificadas, y nunca muestras privadas de terceros).
8. **Cifra tus backups** (restic ya lo hace) y guarda **una copia offline** de tu golden snapshot.
9. **Escribe writeups de todo.** En 6 meses tu cuaderno valdrá más que cualquier curso.

---

## 7. Checklist de entorno (antes de empezar la Fase 1)

- [ ] Windows 11 actualizado, sin restos de OEM innecesarios (deinstalar bloatware acelera)
- [ ] VMware Workstation Pro instalado y arranca VM
- [ ] Kali: 8 GB / 4 vCPU, dos NICs (NAT + host-only 192.168.100.50), `ip -br a` muestra ambas
- [ ] `sudo apt full-upgrade` limpio, `pipx install impacket netexec` funciona (`netexec --version`)
- [ ] `rockyou.txt` descomprimido, `seclists` instalado
- [ ] `open-vm-tools` instalado, clipboard activo en la VM *principal*
- [ ] Snapshot `BASE-LIMPIO` creado
- [ ] VMnet1 en 192.168.100.0/24; ping de Kali a 192.168.100.1 OK
- [ ] WSL2 Ubuntu instalado, `.wslconfig` limitado a 8 GB
- [ ] Docker Desktop instalado, memory limit 6-8 GB, `docker run hello-world` OK
- [ ] Burp Suite Community en Windows, arranca y abre Chromium
- [ ] Wireshark captura en la VMnet1 (captura promiscua)
- [ ] Sysinternals Suite en `C:\Tools\Sysinternals`
- [ ] Ghidra + x64dbg + Wireshark + Autopsy instalados
- [ ] VS Code con extensiones: Python, Pylance, Markdown All in One, GitLens
- [ ] Carpeta `C:\VMs\`, `C:\Tools\`, `C:\Labs\` creadas (fuera de OneDrive)
- [ ] Exclusiones de Defender aplicadas por ruta
- [ ] Bitácora de laboratorio creada: `C:\Labs\notes` + repo Git privado para writeups
