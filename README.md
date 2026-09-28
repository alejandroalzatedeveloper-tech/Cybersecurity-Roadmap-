# 🛡️ Roadmap de Ciberseguridad: de Nivel 0 a Senior

Ruta de estudio completa, paso a paso, adaptada a tu hardware:
**Lenovo ThinkPad P14s Gen 2 · i7-1185G7 (4C/8T) · 48 GB RAM · NVIDIA T500 4 GB · NVMe 954 GB · Windows 11 + Kali en VM**

---

## 📁 Documentos de este kit

| # | Fichero | Contenido |
|---|---|---|
| 01 | [`01-entorno-laboratorio.md`](./01-entorno-laboratorio.md) | Arquitectura del lab, instalación paso a paso de Windows/Kali/WSL2/Docker, configuración óptima de la VM, máquinas de AD, OPSEC, checklist de entorno |
| 02 | [`02-roadmap-por-etapas.md`](./02-roadmap-por-etapas.md) | Las 5 fases (Nivel 0 → Principiante → Intermedio → Avanzado → Senior) con objetivos medibles, temas exactos, recursos, proyectos obligatorios, certificaciones y checklist de dominio |
| 03 | [`03-plan-estudio-y-recursos.md`](./03-plan-estudio-y-recursos.md) | Plan de 52 semanas a 15-20 h/semana, catálogo de recursos gratuitos con enlaces, canales de YouTube, libros, certificaciones y plan a 4 años |

---

## 🚀 Dos versiones de la aplicación

| Fichero | Para qué sirve |
|---|---|
| **`Ruta-Cyber.html`** | **Tu copia personal.** Dimensionada para tu equipo concreto (48 GB de RAM, GPU modesta). Manténla en local. |
| **`Ruta-Cyber-Publica.html`** | **Versión genérica para publicar.** El hardware pasa a ser "configuración de referencia" y quien la lea la adapta a su equipo. Sube esta si quieres compartirla. |
| `Ruta-Cyber.pdf` / `Ruta-Cyber-Publica.pdf` | Versiones impresas (64 y 65 páginas). |

El contenido de la versión pública está en `public/*.md`. Es una copia de los tres
documentos con las partes personales reescritas en impersonal, así que **si editas un
`.md` de la raíz hay que volver a copiarlo a `public/` y reconstruir el HTML** (bloque de
PowerShell de abajo, cambiando `$base` por `$base\public`).

### Qué hace la aplicación

Abre **`Ruta-Cyber.html`** con doble clic. Es un único fichero autocontenido (sin internet, sin Node, sin compilar) que lleva dentro los 3 documentos completos.

| | |
|---|---|
| **Navegación** | Sidebar con los 19 capítulos agrupados por documento y fase |
| **Buscador** | Busca en todo el contenido y salta al capítulo, con las coincidencias resaltadas |
| **Progreso** | 70 checklists marcables; se guardan solos en el navegador (`localStorage`), por dispositivo |
| **Dashboard** | Progreso global, progreso por cada una de las 5 fases y contenido transversal |
| **Plan 52 semanas** | Tu semana actual según la fecha de inicio, con el entregable de cada semana |
| **Modo lector** | Alterna el tema cyberpunk con una vista clara para estudiar |
| **Reiniciar plan** | Botón para recalcular "tu semana" desde hoy |
| **Borrar progreso** | Botón que limpia todos los checks y la fecha de inicio |

Imprimir o exportar a PDF desde el propio navegador: `Ctrl+P`.

### Progreso: cómo funciona

Cada visitante tiene el suyo, siempre empezando en 0%. Se guarda con `localStorage` en
**su propio navegador**, así que:

- **Nada sale del dispositivo.** No hay servidor ni base de datos. El dueño de la página no
  ve el progreso de nadie, y tú no ves el de nadie.
- **No se sincroniza** entre apps, entre ordenadores, ni entre Chrome y Firefox. Se pierde
  si borras los datos de navegación o usas incógnito.
- Cada versión tiene su propia clave, así que tu progreso personal **no** aparece en la
  versión pública:

  | Fichero | Clave `localStorage` |
  |---|---|
  | `Ruta-Cyber.html` | `ruta-cyber-personal-v1` |
  | `Ruta-Cyber-Publica.html` | `ruta-cyber-publica-v1` |

  La clave se fija en el `<meta name="ruta-clave">` de cada HTML, así que se puede cambiar
  en el momento de compilar sin tocar el código.

### Guardar y recuperar el progreso

Como el progreso vive solo en el navegador, la app incluye una copia de seguridad manual.
Está en la tarjeta "Dónde se guarda tu progreso" de la pantalla de inicio:

| Botón | Qué hace |
|---|---|
| **Guardar copia (descargar)** | Descarga `cyber-progreso-AAAA-MM-DD.json` con todo tu estado. |
| **Copiar al portapapeles** | Pega lo mismo en el portapapeles, para moverlo a otro dispositivo. |
| **Cargar desde fichero** | Elige el `.json` y lo aplica. |
| **Fusionar / Reemplazar** | Aplicar el texto pegado, sin fichero. |

**Los dos modos de aplicar, y por qué importan:**

- **Fusionar** (el normal) une lo que hay con lo que llega. Los checks marcados **nunca se
  desmarcan** al sincronizar, se toma la fecha de inicio más antigua y la semana más
  avanzada. Es lo correcto para pasar de un dispositivo a otro.
- **Reemplazar** pisa el estado con el de la copia, exactamente. Útil para restaurar una
  copia antigua completa, porque "fusionar" no puede desmarcar nada.

Por eso el "borrar todo" es un botón aparte: si quisieras que borrar y cargar fueran la
misma operación, el borrado se perdería en la fusión.

**Detalles de diseño que conviene no romper:**

- Los checkboxes se identifican por un hash de su texto, así que el `json` incluye una
  **huella** del contenido. Si editas el Markdown y los checks cambian, al importar salta un
  aviso, y cualquier check de la copia que ya no exista en esta versión se descarta en lugar
  de ensuciar el progreso. Cuántos se han descartado se indica en el mensaje.
- Al importar se **validan los tipos**: un `.json` escrito a mano con `tareas` como texto, o
  `inicio` como `"x"`, se ignoran en lugar de corromper el estado. Es un fallo que se
  detectó probando el ciclo completo, no por la pinta.
- Una copia de la versión personal se puede cargar en la pública y al revés: los 70 checks
  tienen los mismos ids. Solo cambia la clave bajo la que se guarda.
- El fichero es texto plano en JSON. Se puede abrir, leer, editar a mano o guardar en un
  USB, y no contiene nada sensible: solo qué checks marcaste y cuándo empezaste.

### Cómo probar los cambios

Antes de dar por buena una modificación, conviene ejercitar el ciclo completo
(sembrar → exportar → borrar → importar → fusionar → reemplazar → recargar) en un
navegador real. Son 24 comprobaciones y tarda unos segundos; así se detectaron los dos
fallos que había: los checks fantasma y el `Set` al que se le pedía un `sort`.

### `Ruta-Cyber.pdf`

Versión impresa de todo el material (64 y 65 páginas): portada, índice de capítulos, tabla
del plan de 52 semanas y todos los capítulos completos. Para regenerarlas tras editar el
Markdown, ejecuta este bloque. `$src` decide de qué carpeta se lee el contenido (la raíz o
`public\`) y `$clave` es el nombre del `localStorage`, que es lo que mantiene separados los
progresos de las dos versiones.

```powershell
$edge = "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe"
$enc  = New-Object System.Text.UTF8Encoding($false)

function Build($src, $html, $pdf, $clave) {
  $docs = @(
    [ordered]@{ doc='01 · Entorno de laboratorio';       nivel=2; fase=$null; autoFase=$false; txt=[IO.File]::ReadAllText("$src\01-entorno-laboratorio.md",$enc) },
    [ordered]@{ doc='02 · Roadmap por etapas';            nivel=1; fase=$null; autoFase=$true;  txt=[IO.File]::ReadAllText("$src\02-roadmap-por-etapas.md",$enc) },
    [ordered]@{ doc='03 · Plan de estudio y recursos';    nivel=2; fase=$null; autoFase=$false; txt=[IO.File]::ReadAllText("$src\03-plan-estudio-y-recursos.md",$enc) }
  )
  # El .Replace('</','<\/') es obligatorio: sin él, un "</p>" del Markdown cierra
  # antes de tiempo el <script> de datos y el HTML sale roto.
  $json = (ConvertTo-Json -InputObject $docs -Depth 5 -Compress).Replace('</','<\/')
  $out  = [IO.File]::ReadAllText("$PWD\_template.html",$enc).Replace('@@CONTENT_JSON@@',$json).Replace('@@CLAVE@@',$clave)
  if($out -match '@@'){ throw "quedan marcadores sin sustituir en $html" }
  [IO.File]::WriteAllText("$PWD\$html",$out,$enc)

  & $edge --headless=new --disable-gpu --no-sandbox --user-data-dir="$env:TEMP\edge-pdf" `
    --no-pdf-header-footer --virtual-time-budget=25000 `
    --print-to-pdf="$PWD\$pdf" "file:///$($PWD.Path.Replace('\','/'))/$html`?imprimir=1"
}

Build $PWD              'Ruta-Cyber.html'         'Ruta-Cyber.pdf'         'ruta-cyber-personal-v1'
Build "$PWD\public"     'Ruta-Cyber-Publica.html' 'Ruta-Cyber-Publica.pdf' 'ruta-cyber-publica-v1'
```

Los dos marcadores de `_template.html` son `@@CONTENT_JSON@@` (el contenido) y `@@CLAVE@@`
(el nombre del `localStorage`). Si alguna vez compilas una tercera versión, dale su propia
clave y quedará aislada de las otras dos.

**Antes de dar por buena una compilación, compruébalo siempre:**

```powershell
# 1. Sintaxis JS
$h = [IO.File]::ReadAllText("$PWD\Ruta-Cyber.html",$enc)
$s = [regex]::Matches($h,'(?s)<script(?![^>]*application/json)[^>]*>(.*?)</script>')
[IO.File]::WriteAllText("$env:TEMP\check.js", $s[$s.Count-1].Groups[1].Value, $enc)
& "C:\Program Files\nodejs\node.exe" --check "$env:TEMP\check.js"   # debe salir 0

# 2. JSON válido + sin marcadores ni caracteres corruptos
([regex]::Matches($h,'@@')).Count                                    # debe ser 0
([regex]::Matches($h,'[\u4e00-\u9fff\u0400-\u04ff\ufffd]')).Count     # debe ser 0

# 3. PDF correcto
$b = [IO.File]::ReadAllBytes("$PWD\Ruta-Cyber.pdf")
[Text.Encoding]::ASCII.GetString($b[0..7])                           # %PDF-1.4
([regex]::Matches([Text.Encoding]::GetEncoding(28591).GetString($b),'/Type\s*/Page[^s]')).Count
```

Y con el DOM renderizado, que es lo único que revela un PDF en blanco:

```powershell
& $edge --headless=new --disable-gpu --dump-dom `
  "file:///$($PWD.Path.Replace('\','/'))/Ruta-Cyber.html?imprimir=1" > dom.html
# en dom.html deben aparecer class="vista" una vez por capítulo y data-task= una vez por checklist
```

---

## ⚡ Las 3 decisiones que definen tu plan

1. **Tu Constraint #1 es la GPU, no la RAM.** La T500 (4 GB) no sirve para cracking real. Pero 48 GB de RAM sí te permite montar un **Active Directory multi-DC real** en local, que es exactamente lo que el mercado de 2024-2025 exige. Aprovéchalo: prioriza **AD, red, web, blue team y cloud**; deja cracking masivo y reversing pesado para cloud o para cuando tengas GPU.
2. **VMware Workstation Pro es gratis desde mayo de 2024.** No pagues ni busques alternativas: úsalo como hipervisor principal. VirtualBox solo como complemento.
3. **PortSwigger Web Security Academy es tu mayor puerta de entrada** al mercado web. Es gratis, está actualizado, y es literalmente lo que se evalúa en entrevistas y en eJPT/PNPT/CPTS/OSCP. Si solo pudieras hacer una cosa en la Fase 1, es completar sus labs intermedios.

---

## 🗺️ Vista general de las 5 fases

| Fase | Duración | Horas | Resultado | Certificación clave |
|---|---|---|---|---|
| **0 · Cimientos** | 2-3 meses | 130 h | Usas Linux/Windows con soltura, lees tráfico de red, entiendes criptografía | Fortinet NSE 1/2, Cisco, SC-900 (todas gratis) |
| **1 · Principiante** | 4-5 meses | 300 h | Pentest web completo, enumeración, privesc Linux/Windows, primer informe real | eJPT (~€150) |
| **2 · Intermedio** | 5-7 meses | 450 h | Domain Admin en AD, exploit propio, pentest cloud, análisis de malware, detecciones | CPTS (~€200) o eCPT |
| **3 · Avanzado** | 8-12 meses | 800 h | Especialización profunda (Red Team / Detection / Cloud), C2, DFIR avanzado | OSCP / CRTO / CISSP |
| **4 · Senior** | 12-18 meses | 1000 h | Liderazgo, investigación original, CVEs, publicaciones | OSEP / CISSP / GIAC |

**Total: ~2.700 h ≈ 3 años a 20 h/semana.**

---

## ✅ Primeros pasos concretos (esta semana)

1. Lee `01-entorno-laboratorio.md` y ejecuta la **sección 2 (instalación en Windows)**.
2. Aplica la **sección 3 (configuración de la VM Kali)**: 8 GB RAM, 4 vCPU, dos NICs, snapshot `BASE-LIMPIO`.
3. Levanta tu **primer lab de 3 VMs** (sección 5, opción A o B con GOAD-Light).
4. Crea tu **repositorio de notas/writeups** en Git (privado).
5. Apúntate a **PortSwigger Web Security Academy** y resuelve los primeros 5 labs de "Server-Side Request Forgery".
6. Resuelve **OverTheWire Bandit 0-5** para romper la inercia con la terminal.

---

## 📌 Nota legal y de alcance

Todo lo que hagas debe ser sobre **máquinas propias, autorizadas, o plataformas de CTF/lab con autorización explícita** (Hack The Box, TryHackMe, PortSwigger Academy, GOAD, Digital Corpora, etc.). Realizar pruebas de intrusión en sistemas de terceros sin autorización es un delito (art. 264-A LSSI en España; CFAA en EE. UU.; equivalentes en la mayoría de países). Este plan es **exclusivamente para educación y investigación defensiva/autorizada**.

---

*Última actualización: septiembre de 2026 · Recursos verificados y actualizados a 2024-2025*
