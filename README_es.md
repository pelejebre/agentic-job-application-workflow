<!-- prettier-ignore -->

```txt
██████╗ ███████╗██╗     ███████╗     ██╗███████╗██████╗ ██████╗ ███████╗    ██████╗  ███████╗██╗   ██╗
██╔══██╗██╔════╝██║     ██╔════╝     ██║██╔════╝██╔══██╗██╔══██╗██╔════╝    ██╔══██╗ ██╔════╝██║   ██║
██████╔╝█████╗  ██║     █████╗       ██║█████╗  ██████╔╝██████╔╝█████╗      ██║  ██║ █████╗  ██║   ██║
██╔═══╝ ██╔══╝  ██║     ██╔══╝  ██   ██║██╔══╝  ██╔══██╗██╔══██╗██╔══╝      ██║  ██║ ██╔══╝  ╚██╗ ██╔╝
██║     ███████╗███████╗███████╗╚█████╔╝███████╗██████╔╝██║  ██║███████╗██╗ ██████╔╝ ███████╗ ╚████╔╝ 
╚═╝     ╚══════╝╚══════╝╚══════╝ ╚════╝ ╚══════╝╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝ ╚═════╝  ╚══════╝  ╚═══╝
```

<div align="center">
  <h1>Agentic Job Application Workflow</h1>
  <p><em>Automatización inteligente para adaptar tu CV y generar cartas de presentación mediante IA</em></p>
</div>

## Resumen

La búsqueda de empleo en el sector tecnológico exige adaptar el currículum vitae (CV) y las cartas de presentación a los requisitos específicos de cada vacante. Realizar este proceso de forma manual consume mucho tiempo y es propenso a inconsistencias.

Este repositorio implementa un **Toolchain Agéntico** local. En lugar de usar la IA como un simple chatbot, se establece un ecosistema autónomo capaz de leer archivos locales, orquestar múltiples herramientas y ejecutar comandos en segundo plano para producir artefactos en HTML y PDF de alta fidelidad.

> [!NOTE]
> **Privacidad y Veracidad:** Todo el procesamiento se realiza localmente utilizando tu CV maestro. El agente tiene la instrucción estricta de **no inventar** competencias, roles ni certificaciones. Solo reescribe, reorganiza y prioriza tu experiencia real para maximizar la alineación con la vacante.

## Características Clave

- **Análisis de Idoneidad:** Evalúa el encaje entre tu perfil base y la oferta, destacando tus fortalezas y señalando de forma crítica posibles brechas (gaps) a abordar.
- **Adaptación Estratégica (Tailoring):** Reorienta la redacción de tus experiencias previas, competencias digitales y habilidades blandas usando la terminología de la oferta, evitando huecos temporales en tu trayectoria.
- **Maquetación HTML/CSS Dinámica:** Genera documentos responsivos y con calidad de impresión (Pixel-Perfect A4) en dos formatos de plantilla: diseño clásico con barra lateral (V1) o diseño moderno de una columna (V2) con paletas de colores pastel (verde, sepia, azul).
- **Cartas de Presentación ATS:** Redacta cartas persuasivas optimizadas para sistemas ATS en español e inglés, siguiendo pautas profesionales de copywriting.
- **Compilación Headless a PDF:** Renderiza automáticamente los archivos HTML a PDF en segundo plano usando Microsoft Edge o Google Chrome sin necesidad de abrir cuadros de diálogo del sistema.

## Estructura del Proyecto

El núcleo lógico del proyecto reside en el directorio `.agents/`, que contiene las instrucciones maestras y las *Skills*:

```text
agentic-job-application-workflow/
├── data/                                 # Fuente de verdad / Archivos de entrada
│   ├── CV.md                             # Currículum base (Markdown)
│   └── CV_foto.jpg                       # Foto de perfil enlazada por las plantillas
├── analyzed_offers/                      # Carpeta de salida de los documentos generados
│   ├── <ID_OFERTA>_cv.html               # CV adaptado en HTML
│   ├── <ID_OFERTA>_cl.html               # Carta de presentación adaptada en HTML
│   └── <ID_OFERTA>_analisis.md           # Reporte de idoneidad y recomendaciones
└── .agents/                              # Configuración de los agentes
    ├── job_offer_analyzer.md             # Agente orquestador principal
    └── skills/                           # Habilidades modulares invocables
        ├── analysis_generator/           # Genera el reporte de idoneidad y recomendaciones
        ├── cv_generator/                 # Genera el CV adaptado en HTML (diseños V1/V2)
        ├── cover_letter_generator/       # Genera la carta de presentación adaptada en HTML
        └── pdf_printer/                  # Compilador PDF vía Chromium headless
```

## Habilidades Modulares

Cada habilidad es un paquete de ejecución independiente con sus propias plantillas e instrucciones:

- **`analysis_generator`:** Compara tu CV con la oferta, estima el porcentaje de adecuación, detalla las fortalezas y brechas, y redacta una propuesta inicial de carta de presentación.
- **`cv_generator`:** Solicita tus preferencias de diseño, realiza la traducción o adaptación del contenido de tu CV y genera el archivo HTML maquetado.
- **`cover_letter_generator`:** Genera cartas de presentación persuasivas basadas en técnicas de copywriting, adaptadas al puesto e inyectadas en plantillas preparadas para impresión. Se acompaña de una [Guía de Cover Letter](.agents/skills/cover_letter_generator/references/Cover_Letter_Guide.md) con directrices de redacción que el usuario podrá adaptar a su gusto.
- **`pdf_printer`:** Ejecuta un subproceso de Edge/Chrome en modo silencioso (`--headless`) para exportar los documentos HTML para convertirlos en archivos PDF listos para enviar.

## Flujo de Trabajo y Uso

Sigue estos pasos para utilizar el flujo de trabajo:

### 1. Prepara tus Datos de Entrada

Asegúrate de tener tu currículum maestro en `data/CV.md` y tu fotografía en `data/CV_foto.jpg`.

### 2. Inicia el Turno del Agente

Pega el texto de la vacante en el chat del agente indicando un identificador de referencia (por ejemplo, `333333333`):

```markdown
Analiza la siguiente oferta para el ID: 333333333
[Pegar aquí la descripción de la oferta]
```

### 3. Selección de Preferencias y Vista Previa

1. El agente procesará la oferta ejecutando `analysis_generator` y te mostrará el reporte de idoneidad.
2. A continuación, te solicitará confirmar tus preferencias:
   - **Diseño del CV:** Versión 1 (clásico) o Versión 2 (moderno).
   - **Color de fondo (para V2):** Verde claro, sepia o azul.
   - **Idioma de salida:** Español o Inglés.
3. Al recibir tu respuesta, adaptará los documentos y abrirá automáticamente las versiones HTML en tu navegador web predeterminado para que puedas revisarlas.

### 4. Compilación Silenciosa a PDF

Si la previsualización es correcta, confirma al agente para iniciar la compilación. El agente invocará la habilidad `pdf_printer` para exportar a PDF en segundo plano:

```powershell
Start-Process -FilePath "msedge" -ArgumentList "--headless", "--disable-gpu", "--print-to-pdf=\"[RUTA_SALIDA]\"", "--no-margins", "\"[RUTA_ENTRADA]\"" -Wait
```

> [!TIP]
> Si facilitas el nombre del reclutador o del responsable de RRHH en el prompt inicial, el agente generará también un borrador de mensaje de conexión de LinkedIn (máximo 300 caracteres).

## Instalación

Sigue estos pasos para poner en marcha el flujo de trabajo en tu entorno local:

1. **Clonar el repositorio:**

   ```bash
   git clone https://github.com/tu_usuario/agentic-job-application-workflow.git
   cd agentic-job-application-workflow
   ```

2. **Configurar las variables de entorno:**
   Duplica el archivo `.env.example` y renombralo a `.env`:

   ```bash
   cp .env.example .env
   ```

   Abre el archivo `.env` recién creado y completa los datos personales y de contacto del candidato.
3. **Preparar tus datos base:**

   - Coloca tu currículum maestro en formato Markdown en [`data/CV.md`](file:///e:/Documentos/_DEV/agentic-job-application-workflow/data/CV.md). Este archivo sirve como única fuente de verdad para la IA.
   - Guarda tu fotografía profesional en [`data/CV_foto.jpg`](file:///e:/Documentos/_DEV/agentic-job-application-workflow/data/CV_foto.jpg) (la cual será enlazada por las plantillas).
4. **Requisitos de Impresión a PDF:**
   Para compilar de forma desatendida los archivos HTML a PDF mediante la habilidad `pdf_printer`, asegúrate de tener instalado en tu sistema un navegador basado en Chromium:

   - **Windows:** Se garantiza el funcionamiento con Microsoft Edge (instalado de forma nativa).
   - **Otros S.O. / Linux:** Google Chrome o Chromium deben estar accesibles en el PATH de comandos de tu sistema.

## Configuración

Las variables de entorno se definen en el archivo `.env` en la raíz del proyecto:

```env
# Directorio donde se guardarán los análisis y documentos adaptados
OUTPUT_DIR=./analyzed_offers

# Datos de contacto y personales del candidato
CANDIDATE_NAME="Tu Nombre"
CANDIDATE_TITLE="Tu Título Profesional"
CANDIDATE_ADDRESS="Tu Dirección"
CANDIDATE_PHONE="Tu Teléfono"
CANDIDATE_EMAIL="tu_email@ejemplo.com"
CANDIDATE_LINKEDIN="linkedin.com/in/tu_usuario"  # Opcional
CANDIDATE_GITHUB="github.com/tu_usuario"          # Opcional
```
