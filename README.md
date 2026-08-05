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
  <p><em>Intelligent automation to tailor your resume and generate cover letters using AI</em></p>

  <p align="center">
    <a href="#"><img src="https://img.shields.io/badge/Workflow-Agentic-blueviolet?style=flat-square" alt="Agentic Workflow"></a>
    <a href="#"><img src="https://img.shields.io/badge/Shell-Bash%20%7C%20PowerShell-4ae0e1?style=flat-square&logo=gnu-bash" alt="Shell Support"></a>
    <a href="https://developer.mozilla.org/en-US/docs/Web/HTML"><img src="https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white" alt="HTML5"></a>
    <a href="https://developer.mozilla.org/en-US/docs/Web/CSS"><img src="https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white" alt="CSS3"></a>
    <a href="https://www.google.com/chrome/"><img src="https://img.shields.io/badge/PDF-Chromium%20Headless-00a3ee?style=flat-square&logo=google-chrome&logoColor=white" alt="PDF Compilation"></a>
  </p>

  <p align="center">
    ⭐ If you like this project, star it on GitHub — it helps a lot!
  </p>
</div>

## Summary

Job hunting in the tech sector requires tailoring your resume (CV) and cover letters to the specific requirements of each opening. Doing this process manually is highly time-consuming and prone to inconsistencies.

This repository implements a local **Agentic Toolchain**. Rather than using AI as a simple chatbot, it establishes an autonomous ecosystem capable of reading local files, orchestrating multiple tools, and executing background commands to produce high-fidelity HTML and PDF artifacts.

> [!NOTE]
> **Privacy and Veracity:** All processing is done locally using your master resume. The agent has strict instructions **never to invent** skills, roles, or certifications. It only rewrites, reorganizes, and prioritizes your actual experience to maximize alignment with the job vacancy.

## Key Features

- **Suitability Analysis:** Evaluates the fit between your base profile and the job offer, highlighting your strengths and critically pointing out potential gaps (gaps) to address.
- **Strategic Adaptation (Tailoring):** Reorients the phrasing of your previous experiences, digital skills, and soft skills using the terminology of the job offer, avoiding time gaps in your career path.
- **Dynamic HTML/CSS Layout:** Generates responsive, print-quality (Pixel-Perfect A4) documents in two template styles: a classic design with a sidebar (V1) or a modern single-column design (V2) with pastel color palettes (green, sepia, blue).
- **ATS Cover Letters:** Writes persuasive cover letters optimized for ATS systems in both Spanish and English, following professional copywriting guidelines.
- **Headless PDF Compilation:** Automatically renders HTML files to PDF in the background using Microsoft Edge or Google Chrome without opening system dialog boxes.

## Project Structure

The core logic of the project resides in the `.agents/` directory, which contains the master instructions and *Skills*:

```text
agentic-job-application-workflow/
├── data/                                 # Source of truth / Input files
│   ├── CV.md                             # Master resume (Markdown)
│   └── CV_foto.jpg                       # Profile photo linked by templates
├── analyzed_offers/                      # Output directory for generated documents
│   ├── <JOB_ID>_cv.html                  # Tailored CV in HTML
│   ├── <JOB_ID>_cl.html                  # Tailored cover letter in HTML
│   └── <JOB_ID>_analisis.md              # Suitability and recommendations report
└── .agents/                              # Agent configuration
    ├── job_offer_analyzer.md             # Main orchestrator agent
    └── skills/                           # Callable modular skills
        ├── analysis_generator/           # Generates suitability and recommendations report
        ├── cv_generator/                 # Generates tailored CV in HTML (V1/V2 layouts)
        ├── cover_letter_generator/       # Generates tailored cover letter in HTML
        └── pdf_printer/                  # Headless Chromium PDF compiler
```

## Modular Skills

Each skill is an independent execution package with its own templates and instructions:

- **`analysis_generator`:** Compares your resume with the job offer, estimates the matching percentage, details strengths and gaps, and writes an initial cover letter proposal.
- **`cv_generator`:** Requests your layout preferences, translates or adapts your resume content, and generates the styled HTML file.
- **`cover_letter_generator`:** Generates persuasive cover letters based on copywriting techniques, tailored to the position, and injected into print-ready templates. It is accompanied by a [Cover Letter Guide](.agents/skills/cover_letter_generator/references/Cover_Letter_Guide.md) with writing guidelines that the user can customize as desired.
- **`pdf_printer`:** Runs a headless Chrome/Edge subprocess in the background to export HTML documents to ready-to-send PDF files.

## Workflow and Usage

Follow these steps to use the workflow:

### 1. Prepare Your Input Data

Make sure you have your master resume in `data/CV.md` and your professional photograph in `data/CV_foto.jpg`.

### 2. Start the Agent's Turn

Paste the job vacancy text in the agent's chat, specifying a reference ID (for example, `333333333`):

```markdown
Analyze the following job offer for ID: 333333333
[Paste the job description here]
```

### 3. Preferences Selection and Preview

1. The agent will process the job offer by running `analysis_generator` and show you the suitability report.
2. Next, it will ask you to confirm your preferences:
   - **Resume Layout:** Version 1 (classic) or Version 2 (modern).
   - **Background color (for V2):** Light green, sepia, or blue.
   - **Output language:** Spanish or English.
3. Upon receiving your response, it will adapt the documents and automatically open the HTML versions in your default web browser for review.

### 4. Silent PDF Compilation

If the preview looks correct, confirm to the agent to start compilation. The agent will invoke the `pdf_printer` skill to export to PDF in the background:

```powershell
Start-Process -FilePath "msedge" -ArgumentList "--headless", "--disable-gpu", "--print-to-pdf=\"[OUTPUT_PATH]\"", "--no-margins", "\"[INPUT_PATH]\"" -Wait
```

> [!TIP]
> If you provide the name of the recruiter or HR manager in the initial prompt, the agent will also generate a draft LinkedIn connection message (maximum 300 characters).

## Installation

Follow these steps to set up the workflow in your local environment:

1. **Clone the repository:**

   ```bash
   git clone https://github.com/your_username/agentic-job-application-workflow.git
   cd agentic-job-application-workflow
   ```

2. **Configure environment variables:**
   Duplicate the `.env.example` file and rename it to `.env`:

   ```bash
   cp .env.example .env
   ```

   Open the newly created `.env` file and fill in the candidate's personal and contact information.
3. **Prepare your base data:**

   - Place your master resume in Markdown format in [data/CV.md](data/CV.md). This file serves as the single source of truth for the AI.
   - Save your professional photograph in [data/CV_foto.jpg](data/CV_foto.jpg) (which will be linked by the templates).
4. **PDF Printing Requirements:**
   To headlessly compile HTML files into PDFs using the `pdf_printer` skill, ensure a Chromium-based browser is installed on your system:

   - **Windows:** Microsoft Edge is natively supported.
   - **Other OS / Linux:** Google Chrome or Chromium must be accessible in your system's command PATH.

## Configuration

Environment variables are defined in the `.env` file in the project root:

```env
# Directory where analyses and tailored documents will be saved
OUTPUT_DIR=./analyzed_offers

# Candidate's personal and contact information
CANDIDATE_NAME="Your Name"
CANDIDATE_TITLE="Your Professional Title"
CANDIDATE_ADDRESS="Your Address"
CANDIDATE_PHONE="Your Phone"
CANDIDATE_EMAIL="your_email@example.com"
CANDIDATE_LINKEDIN="linkedin.com/in/your_username"  # Optional
CANDIDATE_GITHUB="github.com/your_username"          # Optional
```
