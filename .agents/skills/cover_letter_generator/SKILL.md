---
name: cover_letter_generator
description: Genera una Cover Letter (Carta de Presentación) personalizada en formato HTML adaptada a una oferta de trabajo específica, basándose en la Guía de Cover Letter y guardándola en la carpeta de salida.
compatibility: opencode, antigravity, claude-code
disable-model-invocation: false
user-invocable: true
---

# Skill: Generador de Cover Letter PDF

Esta skill analiza una oferta de trabajo provista por el usuario y la [Cover_Letter_Guide.md](references/Cover_Letter_Guide.md) para redactar una carta de presentación altamente adaptada a la vacante. El resultado se inyecta en una plantilla HTML de una sola columna optimizada para sistemas de selección (ATS).

---

## 📥 Entradas Requeridas

* **Guía de Cover Letter:** Ubicada en `references/Cover_Letter_Guide.md`.
* **Oferta de Trabajo / Vacante:** Texto de la oferta de trabajo provisto por el usuario.
* **ID de la Oferta:** Identificador numérico o alfanumérico provisto por el usuario para organizar los archivos.
* **Color de Fondo / Tema:** Selección de tema (verde, sepia o azul) para mantener consistencia con el CV.
* **Idioma (Español o Inglés):** Idioma en el que se redactará la carta.
* **Datos de Contacto del Reclutador (Opcional):** Nombre y puesto del responsable de selección.
* **Datos del Candidato (.env):** Variables `CANDIDATE_NAME`, `CANDIDATE_TITLE`, `CANDIDATE_ADDRESS`, `CANDIDATE_PHONE`, `CANDIDATE_EMAIL`, `CANDIDATE_LINKEDIN`, and `CANDIDATE_GITHUB`.

---

## ⚙️ Workflow de Ejecución del Agente

Cuando se active esta skill, realiza los siguientes pasos de forma secuencial:

### 1. Lectura de Datos y Análisis
1. Lee las directrices de [Cover_Letter_Guide.md](references/Cover_Letter_Guide.md) (extensión de 250 a 400 palabras, tono asertivo, evitar duplicar literalmente el CV).
2. Extrae los retos de la oferta, palabras clave técnicas y el nombre de la empresa.
3. **Investigación de la Empresa (Condicional):** Si el agente tiene acceso a herramientas de búsqueda web, busca 1-2 noticias o datos recientes de la empresa (lanzamiento de producto, expansión, estrategia publicada, premio, adquisición) e intégralos en el párrafo de Gancho o de Alineación Cultural para demostrar investigación genuina. Si NO tiene acceso web, solicita al usuario que proporcione 1-2 datos concretos sobre la empresa (ej. "¿Puedes compartir algún dato reciente de la empresa como un lanzamiento, noticia o valor corporativo que te haya llamado la atención?"). Si el usuario no proporciona datos, enfócate en los retos del sector y los requisitos del puesto sin inventar información sobre la empresa.

### 2. Redacción Estratégica (Copywriting)
* **Idioma:** Redacta todo en el idioma seleccionado por el usuario.
* **Párrafo 1 (Gancho):** Menciona la vacante exacta y expresa entusiasmo conectando con un logro o con la misión de la empresa.
* **Párrafo 2 (Ajuste Técnico):** Elige 3 logros o habilidades clave reales del candidato (del CV base) directamente vinculados con los requisitos principales de la oferta, y descríbelos aplicando la fórmula: **Verbo de Acción + Palabra Clave + Resultado Cuantificable**. Prioriza aquellos logros que tengan métricas cuantificables.
* **Párrafo 3 (Alineación Cultural):** Explica cómo las competencias del candidato resolverán los retos actuales de la empresa.
* **Propuesta de Contribución a 90 Días (Interactivo — Opcional):** Antes de generar el archivo final, el agente DEBE proponer al usuario 1-2 ideas concretas de contribución que el candidato podría aportar en sus primeros 90 días, basadas en el análisis de la oferta y las fortalezas del CV. Ejemplo: "Podría mencionarse la implantación de un pipeline de automatización con IA para [proceso X de la empresa]." Si el usuario acepta alguna propuesta, intégrala en 1-2 frases dentro del párrafo de Alineación Cultural (Párrafo 3), sin añadir un párrafo separado para no exceder las 400 palabras. Si el usuario las rechaza todas, continúa sin incluir mención a los 90 días.
* **Propuestas de Personalización (Interactivo):** Antes de escribir el archivo HTML final, el agente DEBE presentar al usuario un borrador de la carta con 2-3 propuestas de personalización marcadas como opciones, por ejemplo: (a) Opción A para el gancho: "[Versión enfocada en logro personal]" vs "[Versión enfocada en misión de la empresa]". (b) Propuesta de cierre: "[CTA con mención a disponibilidad inmediata]" vs "[CTA con mención a un proyecto específico]". El usuario elige las opciones que prefiere y el agente genera el archivo final con las elecciones incorporadas. Si el usuario indica que no quiere elegir, el agente selecciona las opciones que considere más efectivas.
* **Párrafo 4 (Cierre y CTA):** Reafirma el interés e invita proactivamente a una entrevista ofreciendo disponibilidad general.

### 3. Generación del Archivo de Salida
1. Carga la plantilla de la Cover Letter: [cl_template.html](templates/cl_template.html).
2. Ajusta la clase del `<body>` según el color elegido:
   - Verde claro: `<body class="theme-verde">`
   - Sepia claro: `<body class="theme-sepia">`
   - Azul claro: `<body class="theme-azul">`
3. Lee las variables del candidato desde el archivo `.env` en la raíz del proyecto.
4. Reemplaza los placeholders del candidato en la plantilla:
   - `[CANDIDATE_NAME]`: Valor de `CANDIDATE_NAME`.
   - `[CANDIDATE_TITLE]`: Valor de `CANDIDATE_TITLE`.
   - `[CANDIDATE_ADDRESS]`: Valor de `CANDIDATE_ADDRESS`.
   - `[CANDIDATE_PHONE]`: Valor de `CANDIDATE_PHONE`.
   - `[CANDIDATE_EMAIL]`: Valor de `CANDIDATE_EMAIL`.
   - `[CANDIDATE_LINKEDIN]`: Valor de `CANDIDATE_LINKEDIN`.
   - `[CANDIDATE_GITHUB]`: Valor de `CANDIDATE_GITHUB`.
5. **Manejo de Campos Vacíos (Opción A):** Si alguna variable opcional (`CANDIDATE_LINKEDIN`, `CANDIDATE_GITHUB`) está vacía o no existe en el `.env`, el agente DEBE eliminar físicamente el elemento HTML correspondiente del archivo final (por ejemplo, el bloque `<div class="contact-item" id="contact-linkedin">` o `<div class="contact-item" id="contact-github">`).
6. Si el idioma es **Inglés**, realiza las siguientes traducciones:
    - Traduce etiquetas de contacto estáticas en la plantilla (ej. cambiar texto en el bloque de dirección o etiquetas si aplica, "Asunto" -> "Subject", "Atentamente" -> "Sincerely"). Asegúrate de formatear la dirección [CANDIDATE_ADDRESS] al estilo inglés si se detecta la abreviatura "C/" para calle (ej. "1 Alameda Hnos. Muñoz St." en vez de "C/Alameda Hnos. Muñoz, 1").
7. Reemplaza los placeholders dinámicos de la oferta en la plantilla:
   - `[FECHA_DE_HOY]`: Fecha de ejecución actual (ej. en inglés: "August 5, 2026"; en español: "5 de agosto de 2026").
   - `[RESPONSABLE_SELECCION]`: Nombre del reclutador (o "Responsable de Selección" / "Hiring Manager" si no se conoce).
   - `[PUESTO_RESPONSABLE]`: Puesto del reclutador (o "Departamento de Adquisición de Talento" / "Talent Acquisition Department").
   - `[EMPRESA]`: Nombre de la empresa.
   - `[TITULO_PUESTO]`: Título exacto de la oferta.
   - `[ID_OFERTA]`: Referencia de la oferta.
8. Lee el valor de `OUTPUT_DIR` desde el archivo `.env` en la raíz del proyecto (por defecto `./analyzed_offers`). Escribe el archivo HTML final en `[OUTPUT_DIR]/<ID_OFERTA>_cl.html` (o `[OUTPUT_DIR]/<ID_OFERTA>_cl_en.html` si es en inglés), resolviendo la ruta relativa a la raíz (ej. `../../../[OUTPUT_DIR]/<ID_OFERTA>_cl.html`).
9. **Apertura automática:** Lanza un comando de sistema (ej. `Start-Process` en PowerShell en Windows) para abrir de forma directa el archivo HTML generado en el navegador web predeterminado del usuario para su revisión inmediata.

---

## 📤 Confirmación y Reporte

Confirma la generación de la carta indicando:
* Ruta del archivo HTML generado: En la carpeta de salida configurada en `.env` (ej. `[OUTPUT_DIR]/<ID_OFERTA>_cl.html`).
* Breve explicación del enfoque persuasivo utilizado en el gancho y en la alíneación cultural.
* Recordatorio para imprimir a PDF activando "Gráficos de fondo".
