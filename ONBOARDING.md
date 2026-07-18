# Guía de Onboarding — Proyecto KiCad Heltec ESP32 LoRa V3

> Documento dirigido a colaboradores del repositorio [`Proyecto-tesis`](https://github.com/carlosdgarcia210/Proyecto-tesis).  
> Sigue los pasos en orden para tener el entorno idéntico al del equipo principal.

---

## Requisitos previos

Antes de comenzar, asegúrate de tener instalado:

| Herramienta | Versión mínima | Verificar con |
|---|---|---|
| Git | 2.x | `git --version` |
| KiCad | 10.x | Abre KiCad → `Ayuda → Acerca de` |

---

## 1. Clonar el repositorio

Abre una terminal y navega a la carpeta donde quieres guardar el proyecto:

```bash
cd ~/Proyectos
git clone https://github.com/carlosdgarcia210/Proyecto-tesis.git
cd Proyecto-tesis
```

Esto descarga todos los archivos, incluyendo las librerías locales del Heltec ESP32 LoRa V3 que ya están incluidas en el repositorio.

---

## 2. Estructura del proyecto

Una vez clonado, verás esta estructura:

```
Proyecto-tesis/
├── .gitignore
├── CARLOS.kicad_pro          ← archivo principal del proyecto
├── CARLOS.kicad_sch          ← esquemático
├── CARLOS.kicad_pcb          ← diseño PCB
├── fp-lib-table              ← tabla de librerías de footprints
├── sym-lib-table             ← tabla de librerías de símbolos
└── libs/
    └── WIFI_KIT_32__V3_/
        ├── Heltec_ESP32_LoRa_V3.kicad_sym       ← símbolo
        ├── Heltec_ESP32_LoRa_V3.pretty/
        │   └── Heltec_ESP32_LoRa_V3.kicad_mod   ← footprint
        └── 3D/
            └── Heltec_ESP32_LoRa_V3.step        ← modelo 3D
```

> **Importante:** Los archivos `fp-lib-table` y `sym-lib-table` ya apuntan a las librerías locales dentro de `libs/`. KiCad los lee automáticamente al abrir el proyecto — **no necesitas registrar las librerías manualmente**.

---

## 3. Abrir el proyecto en KiCad

1. Abre KiCad 10
2. Ve a `Archivo → Abrir Proyecto`
3. Navega a la carpeta `Proyecto-tesis/` y selecciona `CARLOS.kicad_pro`
4. Verifica que el esquemático y el PCB abren sin errores de librerías faltantes

### Verificar el modelo 3D

1. Abre el **Editor de PCB** (`CARLOS.kicad_pcb`)
2. Presiona `Alt + 3` para abrir el visor 3D
3. Deberías ver el modelo del Heltec ESP32 LoRa V3 renderizado

Si el modelo 3D no aparece, ve a la sección de [solución de problemas](#solución-de-problemas) al final de este documento.

---

## 4. Configurar tu identidad en Git (solo la primera vez)

Si es la primera vez que usas Git en tu máquina, configura tu nombre y correo. Esto identifica tus commits:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

---

## 5. Flujo de trabajo diario

### Antes de empezar a trabajar — sincronizar cambios del repositorio

Siempre inicia tu sesión de trabajo bajando los cambios más recientes:

```bash
git pull origin main
```

Esto evita conflictos al subir tu trabajo más tarde.

---

### Después de hacer cambios — subir al repositorio

Cuando termines de modificar archivos en KiCad, guarda todo desde KiCad (`Ctrl + S`) y luego ejecuta en la terminal:

```bash
# 1. Ver qué archivos cambiaron
git status

# 2. Agregar los archivos modificados
git add .

# 3. Crear un commit describiendo qué hiciste
git commit -m "descripción breve de los cambios"

# 4. Subir al repositorio remoto
git push origin main
```

---

### Ejemplo de mensajes de commit recomendados

Usa mensajes descriptivos para que el historial sea claro:

```bash
git commit -m "feat: agregar símbolo del regulador de voltaje en esquemático"
git commit -m "fix: corregir ancho de pistas en capa de cobre"
git commit -m "refactor: reorganizar componentes en el PCB"
git commit -m "docs: actualizar notas del esquemático"
```

---

## 6. Verificar el historial de cambios

Para ver los commits anteriores y saber qué cambió:

```bash
# Ver historial resumido
git log --oneline

# Ver historial con fechas y autores
git log --oneline --graph --all
```

---

## Solución de problemas

### El modelo 3D no aparece en el visor

Esto ocurre si KiCad no resuelve la ruta del archivo `.step`. Verifica lo siguiente:

1. Abre el **Editor de Footprints**
2. Abre el footprint `Heltec_ESP32_LoRa_V3`
3. Ve a `Editar → Propiedades del Footprint → pestaña "Modelos 3D"`
4. Confirma que la ruta dice algo como:
   ```
   ${KIPRJMOD}/libs/WIFI_KIT_32__V3_/3D/Heltec_ESP32_LoRa_V3.step
   ```
5. `${KIPRJMOD}` es reemplazado automáticamente por KiCad con la ruta de tu proyecto — si clonaste correctamente, debería funcionar sin cambios

### Aparece un error de librería faltante al abrir el esquemático

Significa que los archivos `fp-lib-table` o `sym-lib-table` no fueron clonados correctamente. Verifica:

```bash
ls -la
```

Ambos archivos deben aparecer en el listado. Si no están, ejecuta:

```bash
git pull origin main
```

### Git pide usuario y contraseña en cada push

GitHub ya no acepta contraseñas directas. Necesitas un **Personal Access Token (PAT)**:

1. Ve a GitHub → `Settings → Developer settings → Personal access tokens → Tokens (classic)`
2. Genera un token con permisos de `repo`
3. Úsalo como contraseña cuando Git te la pida

Para no tener que ingresarlo cada vez:

```bash
git config --global credential.helper store
```

La próxima vez que ingreses el token, Git lo recordará.

---

## Referencia rápida de comandos

| Acción | Comando |
|---|---|
| Bajar cambios del repo | `git pull origin main` |
| Ver archivos modificados | `git status` |
| Agregar todos los cambios | `git add .` |
| Crear un commit | `git commit -m "mensaje"` |
| Subir cambios | `git push origin main` |
| Ver historial | `git log --oneline` |

---

*Documento generado para el proyecto de tesis — Heltec ESP32 LoRa V3 en KiCad 10.*
