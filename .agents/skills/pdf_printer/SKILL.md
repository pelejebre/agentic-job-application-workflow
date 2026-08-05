---
name: pdf_printer
description: Automatiza la conversión de documentos HTML (CV o Cover Letter) a formato PDF utilizando un navegador Chromium (Edge/Chrome) en modo headless, respetando el paginado A4 exacto y la configuración de gráficos de fondo.
compatibility: opencode, antigravity, claude-code
disable-model-invocation: false
user-invocable: true
---
# Skill: Impresor de CV y Cover Letter a PDF (Headless Chromium)

Esta skill permite generar de forma automática y precisa el archivo PDF final a partir de los documentos en HTML (ej. `333333333_cv.html` o `333333333_cl.html`), garantizando que se respete la paginación A4 exacta y se impriman todos los colores y gráficos de fondo sin necesidad de interacción manual del usuario en el cuadro de diálogo de impresión.

---

## 📥 Entradas Requeridas

* **Ruta del HTML de entrada:** Ruta relativa o absoluta del archivo HTML. Si se usa la configuración por defecto, se encuentra en `[OUTPUT_DIR]/<ID_OFERTA>_cv.html` (ej. `../../../analyzed_offers/333333333_cv.html`).
* **Ruta del PDF de salida:** Ruta absoluta donde se guardará el PDF (por defecto, la misma ubicación pero con extensión `.pdf`).

---

## ⚙️ Workflow de Ejecución

Cuando el usuario dé por buenos los archivos HTML o solicite explícitamente generar el PDF, realiza los siguientes pasos de forma secuencial:

### 1. Ejecución del Comando de Impresión Headless

Utiliza **Microsoft Edge** (que está garantizado en entornos Windows) o **Google Chrome** en modo headless para compilar el HTML a PDF usando la opción `--print-to-pdf`.

Ejecuta el siguiente comando en PowerShell:

```powershell
Start-Process -FilePath "msedge" -ArgumentList "--headless", "--disable-gpu", "--print-to-pdf=\"[RUTA_PDF_SALIDA]\"", "--no-margins", "\"[RUTA_HTML_ENTRADA]\"" -Wait
```

*Nota: La opción `--no-margins` junto con el CSS `@page { size: A4; margin: 0; }` asegura que no haya márgenes adicionales agregados por el navegador que rompan la maquetación.*

### 2. Validación de Salida

1. Verifica si el archivo PDF se ha creado en la ruta esperada.
2. Si es posible, confirma el tamaño del archivo resultante.

### 3. Reporte al Usuario

Presenta un mensaje de éxito con el enlace al PDF generado:

* **Ruta del PDF generado:** `file:///[RUTA_PDF_SALIDA]`
* **Recomendación:** Indica que puede abrir el PDF directamente para revisar el resultado final de la maquetación de dos páginas.
