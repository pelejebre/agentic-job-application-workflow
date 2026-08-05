---
name: cv_generator
description: Genera un CV personalizado en formato HTML (con opción de impresión a PDF) adaptado a una oferta de trabajo específica, guardándolo en la carpeta de salida.
compatibility: opencode, antigravity, claude-code
disable-model-invocation: false
user-invocable: true
---
# Skill: Generador de CV PDF

Esta skill analiza una oferta de trabajo provista por el usuario y el CV base del candidato (`data/CV.md`), realiza una adaptación estratégica del perfil y la experiencia profesional del candidato para alinearlos con los requisitos de la oferta, y genera un archivo HTML maquetado en dos páginas A4 interactivas. El diseño replica el estilo de la plantilla elegida (V1 o V2) y mantiene consistencia en la paleta de colores (verde, sepia o azul).

---

## 📥 Entradas Requeridas

* **CV base del candidato:** Ubicado obligatoriamente en el archivo `../../../data/CV.md`.
* **Oferta de Trabajo / Vacante:** Texto de la oferta de trabajo provisto por el usuario.
* **ID de la Oferta:** Identificador numérico o alfanumérico provisto por el usuario para organizar los archivos.
* **Versión de la Plantilla (V1 o V2):** Selección del diseño del CV.
* **Color de Fondo (solo para V2):** Selección de tema (verde, sepia o azul) que se aplicará al CV.
* **Idioma (Español o Inglés):** Idioma en el que se generará el CV definitivo.
* **Datos del Candidato (.env):** Variables `CANDIDATE_NAME`, `CANDIDATE_TITLE`, `CANDIDATE_ADDRESS`, `CANDIDATE_PHONE`, `CANDIDATE_EMAIL`, `CANDIDATE_LINKEDIN`, and `CANDIDATE_GITHUB`.

---

## ⚙️ Workflow de Ejecución del Agente

Cuando se active esta skill, realiza los siguientes pasos de forma secuencial:

### 1. Interacción inicial y Lectura de Datos

1. **Preguntar al usuario:** Antes de generar el HTML, solicita al usuario:
   * Si prefiere la **Versión 1** (diseño clásico con sidebar azul) o la **Versión 2** (diseño de una columna con fondo pastel).
   * Si elige la Versión 2, qué color de fondo desea: **verde claro**, **sepia claro** o **azul claro**.
   * Si desea el CV en **Español** o en **Inglés**.
2. Lee el contenido completo del CV base en [CV.md](../../../data/CV.md).
3. Extrae las palabras clave, tecnologías, metodologías y habilidades técnicas/blandas críticas de la oferta de trabajo facilitada por el usuario.

### 2. Redacción Estratégica Adaptada (Tailoring)

* **Idioma de redacción:** Escribe todo el contenido adaptado (Sobre Mí, tareas, logros, competencias) en el idioma seleccionado por el usuario. Si se elige inglés, traduce de forma precisa y profesional el perfil del candidato.
* **Título Profesional (Subtítulo):** Mantén siempre el título de identidad del candidato fiel a su esencia (ej. `"Ingeniero Industrial | Especialista en IA aplicada y Automatización"` o en inglés `"Industrial Engineer | Specialist in Applied AI and Automation"`). NO uses el título de la oferta de trabajo directamente como subtítulo.
* **Sobre Mí:** Reescribe el párrafo para enfocar los logros, años de experiencia y la especialización en IA hacia el sector y necesidades de la vacante.
* **Experiencia Profesional:** Reorienta las descripciones de los roles pasados (especialmente las viñetas de tareas y logros) para usar términos afines a la oferta. Resalta las funciones de liderazgo, gestión operativa y automatización con IA según aplique.
  * **CRÍTICO - Evitar Huecos Temporales:** Para transmitir confianza, mantén toda la trayectoria profesional de forma continua sin eliminar puestos anteriores. Si un rol antiguo o menos afín no aporta valor directo (ej. gestión de residuos, consultoría junior en seguros), mantenlo de forma muy condensada: indica el título, la entidad, las fechas y una única línea explicativa centrada en competencias transferibles (liderazgo, optimización de procesos, control de calidad, análisis de datos).
  * **Fórmula de Impacto (XYZ):** Reescribe cada bullet point de logros y responsabilidades usando la estructura: **"Logré [X] → medido por [Y] → haciendo [Z]"**. Ejemplo: "Reduje un 15% los tiempos de respuesta operativa implementando un sistema de automatización con Python y Power Automate". Si el logro en CV.md incluye una métrica concreta (%, €, ahorro de tiempo, tamaño de equipo, presupuesto), utilízala en la fórmula completa. Si NO incluye métrica, reformula usando escala cualitativa ("equipo multidisciplinar de X personas", "proyecto estratégico a nivel corporativo") pero NUNCA inventes cifras, porcentajes ni cantidades que no estén en el CV base.
  * **CRÍTICO - Eliminar Red Flags:** Además de evitar huecos temporales: (a) **Herramientas obsoletas:** Si el CV.md lista tecnologías claramente obsoletas o en desuso que no son relevantes para la vacante, omítelas de la sección de Competencias Digitales o reubícalas al final con menor protagonismo; NO las pongas en primera línea. (b) **Contenido irrelevante:** Elimina o condensa al máximo información que no aporte valor para la vacante concreta (hobbies no profesionales, cursos no relacionados, certificaciones caducadas sin relevancia).
* **Competencias Digitales:** Reordena y prioriza las habilidades técnicas de la plantilla para colocar las tecnologías requeridas por la oferta en primera línea.
* **Densidad de Palabras Clave ATS:** Integra las palabras clave extraídas de la oferta de forma natural a lo largo del CV, especialmente en: (a) El párrafo "Sobre Mí" (al menos 2-3 keywords principales del puesto). (b) Los bullets de Experiencia Profesional (usar los términos exactos de la oferta cuando coincidan con habilidades reales del candidato). (c) La sección de Competencias Digitales (listar con la misma nomenclatura de la oferta). Evita el keyword stuffing: las palabras clave deben fluir de forma natural dentro de oraciones con sentido. Si la oferta usa un término y el CV.md usa un sinónimo, prioriza el término exacto de la oferta (ej. si la oferta dice "Machine Learning" y el CV dice "Aprendizaje Automático", usa "Machine Learning").
* **Verbos de Acción de Alto Impacto:** Reemplaza sistemáticamente verbos débiles o pasivos por verbos de acción fuertes en toda la redacción del CV: "Responsable de" → "Lideré/Dirigí", "Participé en" → "Contribuí a/Co-diseñé", "Encargado de" → "Gestioné/Supervisé", "Trabajé en" → "Desarrollé/Implementé", "Ayudé a" → "Impulsé/Facilité". Prioriza verbos como: Lideré, Diseñé, Optimicé, Implementé, Escalé, Automaticé, Transformé, Negocié, Coordiné, Reduje, Incrementé.
* **Formación Académica (Condensar):** Simplifica la sección educativa mostrando únicamente: título obtenido, institución, fechas y menciones de honor o especialización relevante. NO incluyas descripciones extensas de asignaturas, programas o proyectos académicos salvo que sean directamente relevantes para la vacante.
* **CRÍTICO - Veracidad y No Invención:** NO inventes ninguna competencia, tecnología, framework, herramienta, rol o formación que no esté explícitamente listado en el CV base (`CV.md`). Si la vacante requiere algo no presente en el perfil real del candidato, NO debes inventar que lo conoce; simplemente reestructura o reenfoca las habilidades reales que sí posee.

### 3. Generación del Archivo de Salida

1. Carga la plantilla HTML correspondiente a la elección del usuario para el CV:
   * Versión 1: [cv_template.html](templates/cv_template.html)
   * Versión 2: [cv_template_v2.html](templates/cv_template_v2.html)
2. Si se ha seleccionado la **Versión 2**, ajusta la clase del `<body>` según el color elegido:
   * Verde claro: `<body class="theme-verde">`
   * Sepia claro: `<body class="theme-sepia">`
   * Azul claro: `<body class="theme-azul">`
3. Lee las variables del candidato desde el archivo `.env` en la raíz del proyecto.
4. Reemplaza los placeholders del candidato en la plantilla elegida:
   * `[CANDIDATE_NAME]`: Valor de `CANDIDATE_NAME`.
   * `[CANDIDATE_TITLE]`: Valor de `CANDIDATE_TITLE`.
   * `[CANDIDATE_ADDRESS]`: Valor de `CANDIDATE_ADDRESS`.
   * `[CANDIDATE_PHONE]`: Valor de `CANDIDATE_PHONE`.
   * `[CANDIDATE_EMAIL]`: Valor de `CANDIDATE_EMAIL`.
   * `[CANDIDATE_LINKEDIN]`: Valor de `CANDIDATE_LINKEDIN`.
   * `[CANDIDATE_GITHUB]`: Valor de `CANDIDATE_GITHUB`.
5. **Manejo de Campos Vacíos (Opción A):** Si alguna variable opcional (`CANDIDATE_LINKEDIN`, `CANDIDATE_GITHUB`) está vacía o no existe en el `.env`, el agente DEBE eliminar físicamente el elemento HTML correspondiente del archivo final (por ejemplo, el bloque `<div class="contact-item" id="contact-linkedin">` o `<div class="contact-item" id="contact-github">`).
6. Si el usuario seleccionó **Inglés**, traduce todos los encabezados, etiquetas estáticas y campos de contacto de la plantilla HTML antes de escribir el archivo:
   * "Sobre Mí" -> "About Me"
   * "Experiencia Profesional" -> "Professional Experience"
   * "Formación Académica" -> "Education"
   * "Certificaciones" -> "Certifications"
   * "Competencias Digitales" -> "Digital Skills"
   * "Idiomas" -> "Languages"
   * "Puntos Fuertes" -> "Strengths"
   * "Competencias Transversales" -> "Key Skills" o "Soft Skills"
   * Campos de contacto: "Dirección: [CANDIDATE_ADDRESS]" -> "Address: [CANDIDATE_ADDRESS]", "Teléfono: [CANDIDATE_PHONE]" -> "Phone: [CANDIDATE_PHONE]". Asegúrate de formatear la dirección al estilo inglés: si contiene la abreviatura española "C/" para calle, cámbiala por "St." o "Street" y ajusta el orden (ej. "1 Alameda Hnos. Muñoz St.").
7. Reemplaza las secciones correspondientes de la plantilla elegida del CV con los textos adaptados.
8. Lee el valor de `OUTPUT_DIR` desde el archivo `.env` en la raíz del proyecto (por defecto `./analyzed_offers`). Escribe el archivo HTML final en `[OUTPUT_DIR]/<ID_OFERTA>_cv.html` (o `[OUTPUT_DIR]/<ID_OFERTA>_cv_en.html` si es en inglés), resolviendo la ruta relativa a la raíz del proyecto (ej. `../../../[OUTPUT_DIR]/<ID_OFERTA>_cv.html`).
9. **Apertura automática:** Lanza un comando de sistema (ej. `Start-Process` en PowerShell en Windows) para abrir de forma directa el archivo HTML generado en el navegador web predeterminado del usuario para su revisión inmediata.
10. **Optimización ATS del HTML:** (a) **Plantilla V1 (sidebar):** Asegura que todo el texto del sidebar izquierdo esté también representado de forma accesible en el flujo lineal del HTML (los elementos deben leerse de arriba a abajo en orden lógico en el DOM, independientemente del layout visual CSS). Verifica que las competencias y datos de contacto del sidebar se encuentren dentro de etiquetas semánticas (`<section>`, `<ul>`, `<h2>`) y no solo dentro de `<div>` decorativos. (b) **Ambas plantillas:** No uses imágenes con texto incrustado para secciones críticas. Todos los encabezados, competencias y logros deben ser texto seleccionable. (c) **Acrónimos y siglas:** La primera vez que se menciona una certificación o tecnología con acrónimo, escribe el nombre completo seguido del acrónimo: ej. "Project Management Professional (PMP)", "Natural Language Processing (NLP)".

### 4. Foto de Perfil

* La plantilla ya enlaza por defecto a `CV_foto.jpg` que debe estar ubicado en `data/`. No es necesario copiar ni mover ninguna foto adicional.

### 5. Confirmación y Reporte

Confirma la generación del CV al usuario indicando:

* Ruta del CV HTML generado: En la carpeta de salida configurada en `.env` (ej. `[OUTPUT_DIR]/<ID_OFERTA>_cv.html`).
* Un resumen breve de los principales cambios estratégicos introducidos para adaptar el CV a la vacante.
* Instrucciones sobre cómo abrir el HTML en el navegador, editarlo en vivo si lo desea, e imprimirlo a PDF (activando "Gráficos de fondo" en la ventana de impresión).
