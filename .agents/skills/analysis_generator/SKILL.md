---
name: analysis_generator
description: Genera un reporte de análisis de idoneidad, brechas y recomendaciones de adaptación para una oferta de empleo, guardándolo en formato Markdown en la carpeta configurada en el .env (por defecto, analyzed_offers).
compatibility: opencode, antigravity, claude-code
disable-model-invocation: false
user-invocable: true
---

# Skill: Generador de Análisis de Oferta

Esta skill toma los datos de una oferta de empleo provistos por el usuario, analiza la idoneidad del candidato respecto a dicha oferta utilizando el CV base, y genera un reporte detallado en Markdown basado en una plantilla personalizable.

---

## 📥 Entradas Requeridas

* **Nombre de la Oferta:** Nombre o título del puesto vacante.
* **Empresa:** Nombre de la empresa que ofrece el puesto.
* **Texto de la Oferta:** Descripción completa o requisitos de la oferta de trabajo.
* **ID de la Oferta:** Identificador numérico o alfanumérico de la vacante.
* **CV Base:** El archivo de currículum base del candidato ubicado en `../../../data/CV.md`.

---

## ⚙️ Workflow de Ejecución del Agente

Cuando se active esta skill, realiza los siguientes pasos de forma secuencial:

### 1. Lectura y Análisis Comparativo
1. Lee el currículum base del candidato en `../../../data/CV.md`.
2. Analiza el texto de la oferta provisto:
   - **Detección de Idioma:** Identifica el idioma en el que está redactada la descripción.
   - **Traducción (si aplica):** Si el idioma no es castellano/español, traduce la descripción al castellano/español de forma precisa.
   - Identifica habilidades técnicas y blandas requeridas, años de experiencia, requisitos clave y responsabilidades principales.
3. Evalúa la idoneidad del candidato comparando su experiencia y habilidades con los requisitos de la oferta. Estima un porcentaje de compatibilidad.
4. Identifica fortalezas del candidato (puntos fuertes) y áreas de mejora o faltantes (brechas/gaps).
5. Elabora recomendaciones específicas para adaptar el perfil, la experiencia y las habilidades del CV del candidato a esta oferta.
6. **Desglose ATS de Palabras Clave:** Genera una tabla comparativa keyword-por-keyword: extrae todas las palabras clave técnicas, herramientas, metodologías y competencias mencionadas en la oferta. Para cada una, indica si está presente en el CV base del candidato (✅/❌). Si está presente, recomienda si mantener, priorizar o reformular. Si NO está presente y el candidato NO la posee, indica "No incluir (no está en el CV base)" — nunca recomendar añadir algo que el candidato no conoce. Si NO está presente pero el candidato tiene una competencia equivalente o transferible, sugiere cómo reformular la existente.
7. **Score ATS Estimado:** Calcula una puntuación de compatibilidad ATS desglosada: (a) **Keywords técnicas:** % de palabras clave técnicas de la oferta cubiertas por el CV. (b) **Títulos y roles:** ¿El título del candidato coincide con el solicitado? (Alto/Medio/Bajo). (c) **Años de experiencia:** ¿Cumple el rango solicitado? (Sí/Parcial/No). (d) **Certificaciones:** % de certificaciones requeridas que posee el candidato. (e) **Score global estimado:** Media ponderada expresada como porcentaje (0-100%). (f) **Propuestas de mejora ATS:** Lista de acciones concretas para mejorar el score (ej. "Añadir el acrónimo NLP junto a 'Procesamiento de Lenguaje Natural'", "Mover Python a primera posición en Competencias Digitales"). Nota: Este score es una estimación orientativa, no una puntuación real de un sistema ATS.
8. Redacta un borrador o propuesta inicial de mensaje de presentación (Cover Letter).

### 2. Generación del Archivo de Salida
1. Carga la plantilla de análisis: [analisis_template.md](templates/analisis_template.md).
2. Reemplaza los placeholders en la plantilla con la información generada:
   - `[NOMBRE_OFERTA]`: Nombre de la oferta de trabajo.
   - `[EMPRESA]`: Empresa que lo ofrece.
   - `[ID_OFERTA]`: ID de la oferta.
   - `[IDIOMA_OFERTA]`: Código de dos letras del idioma detectado (ej. `ES`, `EN`, `FR`).
   - `[TEXTO_OFERTA]`: Si la oferta está en español, el texto original. Si está en otro idioma, la traducción al español seguida de un separador y el texto original en su idioma nativo.
   - `[PORCENTAJE_COMPATIBILIDAD]`: Porcentaje estimado de idoneidad (ej. 85).
   - `[PUNTO_FUERTE_1]`, `[PUNTO_FUERTE_2]`, etc.: Fortalezas identificadas.
   - `[BRECHA_1]`, `[BRECHA_2]`, etc.: Brechas de habilidades o experiencia encontradas.
   - `[RECOMENDACION_PERFIL]`: Cómo adaptar el extracto o perfil profesional.
   - `[RECOMENDACION_EXPERIENCIA]`: Cómo enfocar los logros y responsabilidades en la sección de experiencia.
   - `[RECOMENDACION_HABILIDADES]`: Palabras clave o tecnologías que se deben destacar o añadir.
   - `[PROPUESTA_CARTA]`: Texto propuesto para la carta de presentación.
   - `[FECHA]`: Fecha de hoy en formato DD/MM/AAAA.
   - `[TABLA_KEYWORDS_ATS]`: Tabla de desglose keyword-por-keyword con columnas: Palabra Clave (Oferta), ¿En CV? (✅/❌), Recomendación.
   - `[SCORE_ATS_KEYWORDS]`: Porcentaje de keywords técnicas cubiertas.
   - `[SCORE_ATS_TITULO]`: Nivel de coincidencia de título (Alto/Medio/Bajo).
   - `[SCORE_ATS_EXPERIENCIA]`: Cumplimiento de años de experiencia (Sí/Parcial/No).
   - `[SCORE_ATS_CERTIFICACIONES]`: Porcentaje de certificaciones cubiertas.
   - `[SCORE_ATS_GLOBAL]`: Score ATS global estimado (%).
   - `[PROPUESTAS_MEJORA_ATS]`: Lista de acciones concretas para mejorar la compatibilidad ATS.
3. Lee el valor de `OUTPUT_DIR` en el archivo `.env` ubicado en la raíz del proyecto (por defecto es `./analyzed_offers`).
4. Escribe el archivo final en `[OUTPUT_DIR]/<ID_OFERTA>_analisis.md` (siendo la ruta resuelta de forma relativa a la raíz del proyecto, ej. `../../../[OUTPUT_DIR]/<ID_OFERTA>_analisis.md`).

---

## 📤 Confirmación y Reporte

Confirma la generación del reporte indicando:
* Ruta del archivo de análisis generado: En el directorio de salida (ej. `[OUTPUT_DIR]/<ID_OFERTA>_analisis.md`).
* Un resumen muy breve del porcentaje de compatibilidad y el principal punto fuerte detectado.
* Pregunta al usuario si desea proceder con la adaptación automática del CV y la generación de la Cover Letter HTML utilizando las skills correspondientes (`cv_generator` y `cover_letter_generator`).
