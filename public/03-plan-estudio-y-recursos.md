# 03 · Plan de estudio realista (15-20 h/semana) y catálogo de recursos

---

## PARTE A — Plan de 52 semanas (año 1) a 15-20 h/semana

### A.1 Rutina semanal tipo (la plantilla que tienes que repetir)

| Día | Bloque | Duración | Actividad | Tipo |
|---|---|---|---|---|
| **Lunes** | 17:30-19:30 | 2 h | **Teoría + práctica guiada** del tema de la semana (curso/libro/PaperSwigger) | 30 % |
| **Martes** | 17:30-19:30 | 2 h | **Lab práctico** (TryHackMe room, HTB machine, PortSwigger lab, pwn.college) | 50 % |
| **Miércoles** | 17:30-19:30 | 2 h | **Lab práctico** (continuación) o **lectura de código/fuente** | 50 % |
| **Jueves** | 17:30-19:30 | 2 h | **Proyecto de la fase** (escribir el script, montar el lab, redactar) | 50 % |
| **Viernes** | 17:30-18:30 | 1 h | **Writeup + documentación**: escribe lo que hiciste los 4 días anteriores | 20 % |
| **Sábado** | 10:00-13:00 | 3 h | **Bloque largo**: tema nuevo + ejercicio práctico profundo | mixto |
| **Domingo** | 17:00-18:00 | 1 h | **Actualización técnica**: newsletter, informe nuevo, 1 writeup de CTF, notas de ATT&CK | mixto |
| | **TOTAL** | **13-15 h** | | |

**Para llegar a 20 h:** añade un segundo bloque de 2 h el martes o miércoles por la tarde, o un bloque de 2-3 h el domingo por la mañana. Nunca sobre 4 h seguidas sin descanso.

**Reglas de la rutina:**
1. **Nunca**_more de 2 sesiones consecutivas sin escribir algo. Lo que no se escribe, se olvida.
2. ** Siempre** termina la semana con un writeup publicado (aunque sea en tu repo privado).
3. **Snapshots antes de cada lab.** Nunca entres en una máquina sin snapshot.
4. **Sesgo hacia la práctica:** si te pasas 2 horas leyendo sin tocar nada, es un día perdido.

### A.2 Calendario semana a semana (año 1)

#### TRIMESTRE 1 — Fase 0 (semanas 1-12, ~150 h)
| Sem | Foco | Entregable |
|---|---|---|
| 1 | Historia de la seguridad, CIA, terminología. Empieza el repo de notas. | Tabla de 5 vulnerabilidades históricas |
| 2 | **Redes I**: OSI/TCP-IP, IP, subnetting, CIDR, ARP, Ethernet | 20 problemas de subnetting resueltos |
| 3 | **Redes II**: TCP/UDP, puertos, DNS, DHCP, NAT, routing | Diagrama de tu red + `dig`/`nslookup` documentados |
| 4 | **Redes III**: HTTP/HTTPS/TLS, proxies, firewalls, VPN, IDS/IPS | Captura Wireshark de un TLS handshake exegética |
| 5 | **Montar lab de 3 VMs** y romper/abrir puertos | Lab funcionando documentado |
| 6 | **Linux I**: FHS, permisos, chmod/chown, SUID, usuarios | Bitácora de comandos |
| 7 | **Linux II**: grep/awk/sed/find, procesos, systemd, cron, SSH | 40 ejercicios de shell resueltos |
| 8 | **Linux III**: bash scripting + OverTheWire Bandit 0-10 | Bandit 0-10 con writeups |
| 9 | **Windows I**: NTFS, ADS, ACLs, procesos, servicios, registro | Tabla de permisos NTFS |
| 10 | **Windows II**: PowerShell (lo importante) + Event Viewer | Script PowerShell que exporta CSV y programa una tarea |
| 11 | **Sysmon** + instalación + captura de eventos | 20 Event IDs catalogados |
| 12 | **Criptografía** + CryptoHack + revisión del trimestre | CryptoHack: Classical, Encoding, Hashes |

#### TRIMESTRE 2 — Fase 1 parte A (semanas 13-24, ~180 h)
| Sem | Foco | Entregable |
|---|---|---|
| 13 | **Web fundamentals** + HTML/JS mínimo + DevTools | Explicar un flujo HTTP completo |
| 14 | **OWASP Top 10** + PortSwigger Academy: SSRF, SSTI, OS command injection | 8 labs |
| 15 | **SQLi** (la más importante) — intro + UNION + boolean/time blind | 10 labs + 1 writeup largo |
| 16 | **SQLi avanzado** + NoSQL + second-order | 8 labs + writeup |
| 17 | **XSS** — reflected, stored, DOM, blind | 10 labs |
| 18 | **CSRF + Access Control/IDOR + Broken Auth** | 10 labs |
| 19 | **Path Traversal / LFI / RFI / SSRF** + Information disclosure | 10 labs |
| 20 | **Burp Suite a fondo** (Intruder, Repeater, Extensiones) + OWASP ZAP | 3 workflows automatizados |
| 21 | **Nmap a fondo** + enumeración de SMB/LDAP/SNMP | Informe de enumeración de tu lab |
| 22 | **Cracking** (john + hashcat) + Hydration de credenciales en lab | 20 hashes rotos |
| 23 | **Privesc Linux** (linpeas, SUID, cron, sudo, GTFOBins) | 3 máquinas HTB Linux |
| 24 | **Privesc Windows** (winPEAS, token, UAC, servicios) | 2 máquinas HTB Windows + informe PDF del trimestre |

#### TRIMESTRE 3 — Fase 1 parte B (semanas 25-30) + Fase 2 inicio (semanas 31-40, ~190 h)
| Sem | Foco | Entregable |
|---|---|---|
| 25-26 | **PortSwigger restante**: deserialización, smuggling, business logic, JWT, OAuth | 20 labs |
| 27-28 | **Informe profesional de pentest** + **primer informe de pentest real** (HTB Pro Lab o una app DVWA) | Informe PDF de 20 páginas |
| 29-30 | **Repaso + primer AD lab** (montar DC) + BloodHound intro | AD funcionando + Grafo de BloodHound |
| 31-32 | **Kerberos/NTLM** a fondo: Kerberoasting, AS-REP roast, PtH | 3 writeups de ataques Kerberos |
| 33-34 | **DCSync + ACL abuse + GPO abuse + delegation** | Commit de Domain Admin documentado |
| 35-36 | **ADCS (ESC1-8)** + Certipy + detección | Writeup de ADCS |
| 37-38 | **GOAD completo** (levantar y comprometer) | Writeup de 3000+ palabras |
| 39-40 | **Blue team: Sigma, Atomic Red Team, Wazuh** | 15 reglas Sigma probadas |

#### TRIMESTRE 4 — Fase 2 (semanas 41-52, ~180 h)
| Sem | Foco | Entregable |
|---|---|---|
| 41-42 | **Exploit dev**: stack overflow, ROP, pwntools | Exploit propio funcional |
| 43-44 | **Malware analysis estático** (PE/ELF, DiE, capa, FLOSS, .NET) | Informe de malware estático |
| 45-46 | **Malware analysis dinámico** (x64dbg, WinDbg, sandbox) | Informe de malware dinámico |
| 47-48 | **Cloud security** (AWS/Azure) + CloudGoat | Informe de pentest cloud |
| 49-50 | **Kubernetes security** + kube-hunter | Compromiso de un cluster |
| 51-52 | **Forense avanzado** (Volatility, NTFS, timeline) + **KPIs + planificación año 2** | Informe forense completo + plan año 2 |

### A.3 Plan de 4 años a 20 h/semana (1000+ h/año)

```
Año 1:  FASE 0 + FASE 1   (~800 h)  →  Junior pentester / SOC L1
         └ Certs: eJPT o Google CyberCert o SC-900

Año 2:  FASE 2             (~900 h)  →  Mid-level / SOC L2
         └ Certs: CPTS o eCPT o CySA+
         └ 1-2 maquinas HTB Pro Lab, 1 pentest real

Año 3:  FASE 3 (Ruta A/B/C) (~1000 h)  →  Senior engineer
         └ Certs: OSCP / CRTO / CISSP según ruta
         └ 1 CVE / 1 blog técnico / 1 talk

Año 4:  FASE 4             (~1000 h)  → Lead / Principal / Researcher
         └ Certs: OSEP / CISSP / GCFA
         └ 1 paper / 1 OSS tool popular / 1 curso
```

### A.4 Sistema de seguimiento (usa Notion, Obsidian o un simple Markdown)
Crea una base de datos con estas columnas y **actualízala cada domingo**:
```
| Fecha | Horas | Fase | Tema | Recurso | Lab/Proyecto | Writeup (link) | Dificultad (1-5) | Notas |
```
- **KPIs mensuales**: horas acumuladas, labs resueltos, writeups escritos, certificaciones.
- **Revisión trimestral**: ¿estoy en la fase que toca? ¿he hecho todos los entregables? ¿qué se me atasca?
- **Objetivo de ratio**: en 12 meses → **1.200-1.400 h**, **60+ labs/CTFs**, **50+ writeups**, **2-3 certificaciones**, **5+ proyectos completos**.

---

## PARTE B — Catálogo de recursos gratuitos (con enlaces)

> Regla: **el 80 % de tu tiempo debe estar en recursos "hands-on"** (labs, plataformas, writeups). Los cursos teórico son solo el andamiaje.

### B.1 Plataformas de práctica (tu terreno de juego)
| Plataforma | Qué es | Enlace | Notas |
|---|---|---|---|
| **PortSwigger Web Security Academy** | La mejor formación web gratuita del mundo. 17 temas, ~250 labs, writeups oficiales | `https://portswigger.net/web-security` | **EMPIEZA POR AQUÍ. Obligatorio.** |
| **Hack The Box** | Máquinas, retos, Pro Labs, Academy, CVE labs, CPTS exam | `https://www.hackthebox.com` | Requiere suscripción para Pro Labs, pero hay muchas máquinas free. |
| **TryHackMe** | Rutas guiadas, muy buena para el inicio, paths completos | `https://tryhackme.com` | Jr Pentester path, SOC L1 path. |
| **picoCTF / picoGym** | Retos de navegación, niveles Wakeup a through | `https://picoctf.org` | Ideal para principiantes. |
| **OverTheWire** | Wargames SSH classics | `https://overthewire.org/wargames/` | Bandit, Natas, Krypton, Advent of Code. |
| **pwn.college** | Curso de binary exploitation y reversing (gratuito, ASU) | `https://pwn.college` | El mejor para exploit dev. |
| **CyberDefenders** | Retos de forensics y blue team | `https://cyberdefenders.com` | Labs de DFIR. |
| **Blue Team Labs Online (BTLO)** | Retos de detección y DFIR | `https://blueteamlabs.online` | |
| **LetsDefend** | Path de SOC Analyst (hay free tier) | `https://letsdefend.io` | |
| **Splunk BOTS** | Datasets de SIEM para IR (Boss of the SOC) | `https://www.splunk.com/en_us/training` | |
| **OffSec Proving Grounds (PG)** | Máquinas de práctica (hay free tier y مُسلم) | `https://portal.attackdefense.com` | **GRATIS en 2024-2025**, muy parecido a la experience real. |
| **CTFtime** | Calendario de CTFs, equipos | `https://ctftime.org` | Registrate y participa en CTFs online. |
| **VulnHub** | Máquinas virtuales vulnerables para descargar | `https://www.vulnhub.com` | Perfecto para tu lab local. |

### B.2 Recursos de web / AppSec
- **OWASP Web Security Testing Guide (WSTG)**: `https://owasp.org/www-project-web-security-testing-guide/`
- **OWASP Cheat Sheet Series** (el mejor punto de referencia rápida): `https://cheatsheetseries.owasp.org/`
- **OWASP Top 10** (y **Top 10 for LLM Apps 2025**): `https://owasp.org/www-project-top-10/`
- **PortSwigger Research Blog** (los mejores writeups de web): `https://portswigger.net/research`
- **books.bugcrowd.com** (Web Security Handbook online gratis): `https://books.bugcrowd.com`
- **HackTricks** (compendio gigantesco de todo, gratis, muy útil de referencia): `https://book.hacktricks.wiki` · repo: `https://github.com/carlosrodicio/hacktricks`
- **PayloadsAllTheThings** (cheat sheets de payloads, de referencia diaria): `https://github.com/swisskyrepo/PayloadsAllTheThings`
- **0x00ffsec** (writeups completos de HTB, los mejores): `https://0x00ffsec.com`
- **The Hacker Recipes** (`https://thehacker.recipes`)
- **PentestMonkey** (tiene contenido gratuito visible aunque el curso sea de pago): `https://pentestmonkey.com`
- **SecLists** (wordlists): `https://github.com/danielmiessler/SecLists`

### B.3 Recursos de red
- **Kurose & Ross Top-Down** (PDF gratis del autor): `https://gaia.cs.umass.edu/kurose_ross/index.php`
- **PacketLife Cheat Sheets**: `https://packetlife.net/campus/cheat-sheets/`
- **NetSecNotes** (diagramas): `https://www.netsecnotes.com`
- **Wireshark User Guide** (oficial): `https://www.wireshark.org/docs/`
- **Prof. Messer (Network+/Windows server)**: `https://www.professormesser.com`
- **NetworkChuck**: `https://www.youtube.com/@NetworkChuck`
- **David Bombal** (labs de red gratis): `https://www.youtube.com/@davidbombal`
- **Practical Networking Handbook** (gratis online): `https://www.practicalnetworking.com` (busca el PDF más reciente)

### B.4 Recursos de Active Directory (AD)
- **ired.team** (notas de ataque a AD, de S4vitar — **EXCELENTE**): `https://ired.team`
- **GOAD (Orange Cyberdefense)**: `https://github.com/Orange-Cyberdefense/GOAD`
- **AD-Miner**: `https://github.com/Orange-Cyberdefense/AD-Miner`
- **Orange Cyberdefense research blog**: `https://www.orangecyberdefense.com/blog/`
- **BloodHound (SpecterOps)**: `https://github.com/SpecterOps/BloodHound` · guía: `https://bloodhound.specterops.io`
- **SharpHound Rules**: `https://github.com/ZeroLegion/SharpHoundRules`
- **BloodHound Useful Queries**: `https://github.com/BloodHoundAD/BloodHound/wiki/Useful-Queries`
- **Orange Cyberdefense: "Adversary Emulation with GOAD"**: parte de su blog
- **Ansible for AD (automation)**: `https://github.com/Orange-Cyberdefense/GOAD/tree/ansible-ad`
- **ADSecurity.org** (escrita por Timothy Jones, de Microsoft): `https://adsecurity.org`
- **SANS whitepapers sobre AD**: `https://www.sans.org/white-papers/`
- **certipy (ESC1-8)**: `https://github.com/ly4k/Certipy`
- **Rubeus**: `https://github.com/GhostPack/Rubeus`
- **CESP (Pivoting)**: `https://github.com/Orange-Cyberdefense/GOAD`
- **orange Cyberdefense GOAD writeups** y **S4vitar's Blog**: `https://s4vitar.github.io` (S4vitar también es el autor de ired.team)

### B.5 Recursos de Exploit Development / Binary
- **pwn.college**: `https://pwn.college`
- **OpenSecurityTraining2** (gratis, x86-64): `https://www.ostraining.org`
- **Practical RE** (Dennis Andriesse, repo del libro): `https://github.com/denisandriesse/practical-re`
- **Exploit Education**: `https://exploit.education`
- **Irrelevant (Narnia, protostar, etc.)**: `https://irrelevant.ch`
- **Hacking: The Art of Exploitation** (el mejor libro de exploit dev, de pago pero imprescindible): `https://www.nostarch.com/hacking-the-art-of-exploitation-2nd-edition/`
- **Buffer Overflow Prep (NCC)**: `https://guyinatuxedo.github.io/`
- **Shellphish (repo legal)**: `https://github.com/shellphish/shellphish`
- **pwntools docs**: `https://docs.pwntools.com`

### B.6 Recursos de malware analysis / reversing
- **Ghidra** (NSA, gratis, profesional): `https://github.com/NationalSecurityAgency/ghidra`
- **Cutter/Rizin** (libre): `https://cutter.re` · `https://rizin.re`
- **x64dbg**: `https://x64dbg.com`
- **capa** (Mandiant): `https://github.com/mandiant/capa`
- **FLOSS**: `https://github.com/mandiant/flare-floss`
- **Detect It Easy**: `https://github.com/horsicq/Detect-It-Easy`
- **PEStudio**: `https://winitor.com`
- **VirusTotal** (gratis para análisis): `https://www.virustotal.com`
- **MalwareBazaar** (samples gratis de malware): `https://bazaar.abuse.ch`
- **Triage** (sandbox gratis): `https://tria.ge`
- **ANY.RUN** (sandbox gratis con límites): `https://any.run`
- **CAPE Sandbox** (puedes auto-hospedar): `https://github.com/kevoreilly/CAPE`
- **Practical Malware Analysis** (paid pero imprescindible) — se consigue en bibliotecas o de segunda mano
- **Google SRE book** (gratis online, excelente): `https://sre.google/sre-book/table-of-contents/`
- **dfir.report** (reports de incidentes reales, con IOCs): `https://dfir.report`
- **Malware Analysis Principles and Practices** (no gratis, pero es la biblia)
- **Internet Storm Center (SANS)**: `https://isc.sans.edu`

### B.7 Recursos de Blue Team / Detection
- **MITRE ATT&CK**: `https://attack.mitre.org` · Navigator: `https://mitre-attack.github.io/attack-navigator/`
- **MITRE D3FEND**: `https://d3fend.mitre.org`
- **MITRE Engage**: `https://engage.mitre.org`
- **Sigma**: `https://github.com/SigmaHQ/sigma`
- **YARA**: `https://github.com/VirusTotal/yara`
- **Atomic Red Team**: `https://github.com/redcanaryco/atomic-red-team`
- **MITRE Caldera**: `https://github.com/mitre/caldera`
- **Stratus Red Team** (cloud): `https://github.com/redcanaryco/stratus-red-team`
- **Cloud-Red-Team** (AWS): `https://github.com/salesforce/Cloud-Red-Team`
- **Elastic detection rules**: `https://github.com/elastic/detection-rules`
- **Splunk Security Content**: `https://security.splunk.com`
- **The Hunting Handbook** (Robert M. Lee, gratis): `https://www.happyhacking.co/hunting-handbook`
- **Wazuh**: `https://wazuh.com`
- **Velociraptor** (DFIR OS): `https://github.com/Velocidex/velociraptor`
- **TheHive** (gestión de incidentes OSS): `https://github.com/TheHive-Project`
- **MISP** (inteligencia de amenazas OSS): `https://github.com/MISP/MISP`
- **Shuffle** (SOAR OSS): `https://github.com/Shuffle/Shuffle`
- **SANS Reading Room** (whitepapers gratuitos de calidad): `https://www.sans.org/reading-room/`
- **Red Canary Threat Detection Hub** (descarga de packs Sigma/YARA): `https://redcanary.com/threat-detection-hub`
- **AlienVault OTX**: `https://otx.alienvault.com` (gratis)

### B.8 Recursos de Forense
- **Sleuth Kit / Autopsy**: `https://www.sleuthkit.org` · `https://www.autopsy.com`
- **Volatility 3**: `https://github.com/volatilityfoundation/volatility3`
- **KAPE**: `https://github.com/KAPE/KAPE`
- **Cyber Triage**: `https://cybertriage.com`
- **Plaso/log2timeline**: `https://github.com/log2timeline/plaso`
- **Timesketch**: `https://github.com/google/timesketch`
- **Digital Corpora** (imágenes forenses): `https://digitalcorpora.org`
- **NIST CFReDS** (imágenes forenses del NIST): `https://cfreds.nist.gov`
- **Cellebrite** (tiene material público), **ForensicFocus**: `https://www.forensicfocus.com`
- **Velociraptor** (ya listado): `https://docs.velociraptor.app`

### B.9 Recursos de Cloud Security
- **AWS Skill Builder** (gratis): `https://skillbuilder.aws`
- **Google Cloud Skills Boost** (free tier): `https://cloudskillsboost.google`
- **Microsoft Learn** (gratis): `https://learn.microsoft.com/es-es/training/`
- **CloudGoat** (AWS): `https://github.com/WithSecureLabs/cloudgoat`
- **Verrus** (AWS/Azure): `https://github.com/salesforce/verrus`
- **pwncloud** (AWS): `https://github.com/RhinoSecurityLabs/pwncloud`
- **Kubernetes Security** (documentación): `https://kubernetes.io/docs/concepts/security/`
- **kube-hunter**: `https://github.com/aquasecurity/kube-hunter`
- **KICS** (IaC scanning): `https://github.com/Checkmarx/KICS`
- **kube-bench** (CIS benchmarks): `https://github.com/aquasecurity/kube-bench`
- **Prowler** (cloud compliance): `https://github.com/prowler-cloud/prowler`
- **Awesome Cloud Security**: `https://github.com/irleks/awesome-cloud-security`
- **Kubernetes Academy / LFS258** (gratis): `https://training.linuxfoundation.com/training/`

### B.10 Recursos de criptografía
- **CryptoHack** (interactivo, gratis): `https://cryptohack.org`
- **cryptopals**: `https://cryptopals.com`
- **Practical Cryptography** (Daniel Boneh & Pratik Prakash, gratis online): `https://practicalcryptography.com`
- **A Graduate Course in Applied Cryptography** (Dan Boneh, PDF gratis): `https://crypto.stanford.edu/~dabo/courses/OnlineCrypto/`
- **Coursera Cryptography I/II** (Dan Boneh, audit gratis): `https://www.coursera.org/learn/cryptography`
- **Modern Cryptography** (Kokubo, abierto): `https://book.masden.org` (busca el repo oficial)
- **PortSwigger "Encryption Attacks"** (labs): parte de la Web Security Academy
- **CryptoHack + CryptoPals** juntos cubren el 80 % de lo que necesitarás

### B.11 Canales de YouTube recomendados
| Canal | Idioma | Contenido |
|---|---|---|
| **NetworkChuck** | EN | Redes, hacking, Docker, Linux (energía alta) |
| **John Hammond** | EN | Malware, CTFs, pentest, threat actor reports |
| **IppSec** | EN | Walkthroughs de HTB (los mejores writeups en vídeo) |
| **LiveOverflow** | EN | Teoría de bajo nivel, cripto, exploiting |
| **The Cyber Mentor** | EN | Harness/eJPT, pentesting práctico, fundamentos ofensivos |
| **Professor Messer** | EN | Network+, Linux, Windows Server (fundamentos sólidos) |
| **David Bombal** | EN | Redes, Kali, labs practicales |
| **HackerSploit** | EN | Practical hacking, pentest práctico |
| **0xFFSEC** | EN | Writeups de HTB |
| **PwnFunction** | EN | Exploit dev, pwn, pwntools (excelente) |
| **LowLevelLearning** | EN | x86-64 assembly, exploit dev |
| **S4vitar** | ES | Android/Flutter security, OSINT, AD (el ired.team es suyo) |
| **Demente0** | ES | Explotación binaria, pwn, ROP (en español, muy bueno) |
| **Machines Learning** | EN | Con IA, research |
| **Kevin Beato / Vergilius Project** | EN | Ghidra, reversing |
| **Simplify by simpleX** | EN | Programación, pero útil para scripts de pentest |

**Canales en español de referencia (calidad variable, peroJX verify antes de studying):**
- **S4vitar** (España) — el más serio y técnico
- **Demente0** (España) — binario/exploit dev
- **El Instructor / Juan Ignacio** (España) — hacking ético general
- **Ciberseguridad CCN-CERT** — videos de concienciación
- **INCIBE** — videos y guías en español

### B.12 Certificaciones gratuitas y de bajo coste (recompiladas)
| Certificación | Coste | Cuándo |
|---|---|---|
| **Fortinet NSE 1 + NSE 2** | 100 % gratis | Fase 0-1 |
| **Cisco Introduction to Networking (Skills for All)** | Gratis | Fase 0 |
| **Microsoft SC-900** | Gratis (con Microsoft Learn) | Fase 0-1 |
| **Palo Alto Cybersecurity Academy 10x01/10x11** | Gratis | Fase 1 |
| **Check Point CC** | Gratis | Fase 1 |
| **AWS Cloud Practitioner** | Gratis (exam voucher) | Fase 2 |
| **Google Cloud Digital Leader** | Gratis (exam voucher) | Fase 2 |
| **Kubernetes Fundamentals (LFS258)** | Gratis (Linux Foundation/edX) | Fase 2 |
| **Introduction to Linux (LFS158x)** | Gratis (Linux Foundation/edX) | Fase 0 |
| **SANS CyberDefenders** (labs) | Gratis | Fase 1-2 |
| **SANS Reading Room** | Gratis | Siempre |
| **Google Cybersecurity Professional Certificate** | ~$49/mes (7 meses) | Fase 0-1 |
| **eJPT (INE)** | ~$100-250 | Fase 1 |
| **CPTS (Hack The Box)** | ~$210 | Fase 1-2 |
| **eCPT (INE)** | ~$400 | Fase 2 |
| **CompTIA Security+ / PenTest+ / CySA+** | ~$350-500 (vouchers bajan a $150) | Fase 1-2 |
| **CRTO (ZeroPointSecurity)** | ~$500 | Fase 3 |
| **OSCP (OffSec)** | ~$1599 + 30 días | Fase 3 |
| **OSEP (OffSec)** | ~$2000 (becas: ~$500) | Fase 4 |
| **CISSP / CISM** | ~$700-1000 (5 años exp) | Fase 4 |

### B.13 Libros recomendados (los mínimos imprescindibles)
| Libro | Tema | Gratis/Precio |
|---|---|---|
| **Kurose & Ross — Top-Down Networking** | Redes | Gratis (autor) |
| **The Linux Command Line** (Shotts) | Linux | Gratis (`linuxcommand.org`) |
| **Linux From Scratch** | Linux profundo | Gratis (`linuxfromscratch.org`) |
| **Practical Cryptography** (Boneh) | Cripto | Gratis online |
| **The Web Application Hacker's Handbook** (Stuttard & Pinto) | Web pentest | Libro (la web está por partes: usa PortSwigger Academy) |
| **Hacking: The Art of Exploitation** (Stevens & Aitel) | Exploit dev | Libro (esencial) |
| **The Hacker's Handbook** | General | Libro (clásico) |
| **Red Team Field Manual (RTFM)** | Red team ops | Libro (libre) |
| **Blue Team Handbook: Incident Response Edition** | Blue team / IR | **Gratis** (PDF, de DFIR Report) |
| **Hunting Handbook** (R. M. Lee) | Threat hunting | Gratis |
| **Practical Packet Analysis** (Wireshark) | Análisis de red | Libro |
| **Malware Analysis: The Big Picture** | Malware | **Gratis** (VirusTotal) |
| **Secure Coding 101** | General | Multiple (OWASP) |
| **The Art of Computer Virus Research and Defense** | Malware | Libro (esencial en malware) |

### B.14 Referencias de normativa / Compliance
- **CIS Benchmarks**: `https://www.cisecurity.org/benchmark`
- **CIS Controls**: `https://www.cisecurity.org/controls`
- **NIST Cybersecurity Framework 2.0**: `https://www.nist.gov/cyberframework`
- **ISO 27001/27701** (implementación, no necesitas el estándar)
- **ENS (España)**: `https://www.incibe.es/seguridad-ciber/ens`
- **NIS2**: `https://digital-strategy.ec.europa.eu`
- **DORA** (resiliencia financiera): `https://www.esma.europa.eu`
- **CSA (Cloud Security Alliance)**: `https://cloudsecurityalliance.org`

---

## PARTE C — Cómo no abandonar (la parte que nadie cuenta)

1. **El enemigo real es el "learn-while-doing" mal gestionado:** Estate 5 horas leyendo sin tocar nada y te sientes productivo, pero no avanzas. **Solución:** la regla del 50/50 en la sesión.
2. **Es un maratón de 3 años.** La constancia gana a la intensidad. 2h/día > 12h el domingo.
3. **Las mesetas son normales.** Viene un día donde no entiendes Kerberos, o donde un lab te lleva 5 h. **La solución:** mira la solución/writeup, entiende por qué no lo viste, anótalo, sigue. No abandones la semana.
4. **No busques la perfección.** Entiende el 70 % de un tema, pasa al siguiente, vuelve a los 3 meses. El relleno se rellena.
5. **Documenta desde hoy.** Tu repo de notas es lo que te va a diferenciar en una entrevista.
6. **Busca comunidad:** en Twitter/X con #cybersecurity, en Discord de HTB/THM, en r/netsec, en r/ReverseEngineering, y en **foros**. Estudiar solo es más lento.
7. **Compromiso de entrevista:** cuando llegues a Fase 2, empieza a hacer entrevistas aunque no te sientas listo. La práctica de entrevistas es una habilidad propia.
8. **No te aferrantes a la certificación al principio.** Las certificaciones no importan nada hasta la Fase 2-3. Antes, conocimiento puro.
9. **Automatiza lo repetitivo:** cada tarea repetida (encender VMs, montar snapshot, bajar labs) debería ser un script. Empieza AHORA.
10. **Celebra los hitos:** cada 20 labs, cada 100 h, cada cert → haz algo que te guste. La sostenibilidad importa.

---

## PARTE D — Resumen ejecutivo (para tener a mano)

```
╔══════════════════════════════════════════════════════════════════╗
║  HARDWARE DE REFERENCIA: CPU 4-8 nuclear + 32-48 GB RAM         ║
║  LIMITACION HABITUAL: GPU modesta → no hace falta para cracking║
║  VENTAJA TIPICA: 32-48 GB RAM → permite AD multi-DC real.       ║
╠══════════════════════════════════════════════════════════════════╣
║  ARQUITECTURA:                                                  ║
║    Kali (8GB/4vCPU) + DC01 (4GB) + DC02 (4GB) + CA01 (4GB)     ║
║    + VulnHub/HTB (2-4GB)  = ~26 GB VMs + 11 GB host            ║
║    + WSL2 (8GB) + Docker (6-8GB) →respeta la suma              ║
╠══════════════════════════════════════════════════════════════════╣
║  FASE 0: 130h  → Red, Linux, Windows, Python, cripto           ║
║  FASE 1: 300h  → Web pentest, enumeración, privesc, primOS INTRO ║
║  FASE 2: 450h  → AD profundo, exploit dev, cloud, malware, DT   ║
║  FASE 3: 800h  → Especialización (Red/Blue/Cloud) + leadership   ║
║  FASE 4: 1000h → Investigación, liderazgo, publicaciones         ║
║  TOTAL:  ~2680h ≈ 3 años a 20h/semana                            ║
╠══════════════════════════════════════════════════════════════════╣
║  REGLA DE ORO: 30% teoría / 50% práctica / 20% escritura        ║
║  PRIMERA ACCIÓN: Monta el lab de AD, sube PortSwigger Academy.   ║
╚══════════════════════════════════════════════════════════════════╝
```
