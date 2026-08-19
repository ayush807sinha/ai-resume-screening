# AI Resume Screening & Job Matching System

An AI-powered command-line application that automatically analyzes a **Job Description**, parses multiple candidate resumes, compares candidates against the job requirements, generates a **match score**, and ranks candidates based on their relevance.

The project uses the **Groq API** and an LLM to extract structured information from unstructured job descriptions and resumes.

---

## 🚀 Features

* Analyze and structure Job Descriptions using AI.
* Read resumes in **PDF** and **DOCX** formats.
* Extract candidate:

  * Name
  * Email
  * Phone
  * Skills
  * Work Experience
  * Education
  * Projects
  * Certifications
* Extract job:

  * Role
  * Required Skills
  * Preferred Skills
  * Minimum Experience
  * Educational Requirements
  * Responsibilities
* Compare resumes against Job Descriptions.
* Generate an **overall match score from 0–100**.
* Identify matching skills.
* Identify missing important skills.
* Check whether required experience is met.
* Generate a short recruiter-style verdict.
* Automatically rank candidates based on their match score.
* Run everything through the command line.

---

# 🛠️ Tech Stack

* **Python**
* **Groq API**
* **OpenAI GPT-OSS 120B**
* **Pydantic**
* **PyPDF**
* **python-docx**
* **python-dotenv**
* **uv**

---

# 📁 Project Structure

```text
ai-resume-screening/
│
├── .gitignore
├── .python-version
├── README.md
├── miniProject.py
├── pyproject.toml
├── uv.lock
│
├── resume/
│   ├── candidate1.pdf
│   ├── candidate2.pdf
│   └── candidate3.docx
│
└── src/
    └── ...
```

### Supported Resume Formats

The application currently supports:

```text
.pdf
.docx
```

Place candidate resumes inside the `resume/` directory.

---

# ⚙️ Installation & Setup

## 1. Install uv

This project uses **uv** for Python project and dependency management.

### Windows

Open PowerShell and run:

```powershell
pip install uv
```

Verify the installation:

```powershell
uv --version
```

---

# 2. Clone the Repository

Clone the project from GitHub:

```powershell
git clone https://github.com/ayush807sinha/ai-resume-screening.git
```

Move into the project directory:

```powershell
cd ai-resume-screening
```

---

# 3. Recommended Setup for the Existing Project

Because this repository already contains `pyproject.toml` and `uv.lock`, the easiest way to install the project is:

```powershell
uv sync
```

This will:

* Create the virtual environment.
* Install the required dependencies.
* Use the dependency versions stored in `uv.lock`.

Activate the virtual environment:

```powershell
.venv\Scripts\activate
```

---

# 🆕 Setting Up the Project from Scratch

If you want to create the project yourself from an empty directory, use the following steps.

## Initialize the uv project

```powershell
uv init
```

Create the virtual environment:

```powershell
uv venv
```

Activate it:

```powershell
.venv\Scripts\activate
```

---

# 📦 Install Dependencies

Install the required packages using `uv add`.

### Groq

```powershell
uv add groq
```

### Environment Variables

```powershell
uv add python-dotenv
```

### Pydantic

```powershell
uv add pydantic
```

### PDF Reader

```powershell
uv add pypdf
```

### DOCX Reader

```powershell
uv add python-docx
```

### Install Everything at Once

You can also install all dependencies with one command:

```powershell
uv add groq python-dotenv pydantic pypdf python-docx
```

These dependencies will be recorded in `pyproject.toml` and `uv.lock`.

---

# 🔑 Groq API Configuration

The application uses the Groq API for AI-powered Job Description and resume analysis.

Create a file named:

```text
.env
```

in the root directory:

```text
ai-resume-screening/
│
├── .env
├── miniProject.py
├── pyproject.toml
└── ...
```

Add your Groq API key:

```env
GROQ_API_KEY=your_groq_api_key_here
```

Replace `your_groq_api_key_here` with your actual API key.

### ⚠️ Important

**Never upload your ****`.env`**** file to GitHub.**

The `.gitignore` file should contain:

```gitignore
# Python-generated files
__pycache__/
*.py[oc]
build/
dist/
wheels/
*.egg-info/

# Virtual environment
.venv/

# Environment variables / secrets
.env
.env.*
!.env.example
```

You can optionally create a `.env.example` file:

```env
GROQ_API_KEY=your_groq_api_key_here
```

This file can safely be committed to GitHub because it does not contain a real API key.

---

# 📄 Adding Candidate Resumes

Create a `resume` folder in the project root:

```text
resume/
```

Add candidate resumes:

```text
resume/
├── candidate1.pdf
├── candidate2.pdf
├── candidate3.docx
└── candidate4.pdf
```

The application automatically scans this folder.

You do not need to manually provide every resume filename.

---

# 📝 Job Description

The Job Description is provided to the application and analyzed using the Groq LLM.

The system extracts information such as:

* Job Role
* Required Skills
* Preferred Skills
* Minimum Experience
* Educational Requirements
* Responsibilities

For example:

```text
Software Development Engineer

Required Skills:
Java
Spring Boot
REST APIs
MySQL
React.js
Microservices

Experience:
0-2 years
```

The LLM converts the unstructured Job Description into structured data using a Pydantic model.

---

# 🧠 How the System Works

The application follows this pipeline:

```text
              Job Description
                     │
                     ▼
                 Groq LLM
                     │
                     ▼
           Structured Job Data
                     │
                     │
                     ▼
              Resume Folder
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
      Resume PDF            Resume DOCX
          │                     │
          └──────────┬──────────┘
                     ▼
              Text Extraction
                     │
                     ▼
               AI Resume Parser
                     │
                     ▼
           Structured Resume Data
                     │
                     ▼
            Resume vs Job Match
                     │
                     ▼
             Match Score 0-100
                     │
                     ▼
             Candidate Ranking
```

---

# 🔍 Step 1 — Job Description Analysis

The Job Description is sent to the Groq LLM.

The application uses a Pydantic model to define the expected structure.

The model extracts:

```text
Role
Required Skills
Preferred Skills
Minimum Experience
Educational Requirements
Responsibilities
```

Example structured output:

```json
{
  "role": "Software Development Engineer",
  "required_skills": [
    "Java",
    "Spring Boot",
    "REST APIs"
  ],
  "prefered_skills": [
    "React.js",
    "Microservices"
  ],
  "minimum_exp": 1,
  "educational_requirements": [
    "Bachelor's degree in Computer Science"
  ],
  "resposibilities": [
    "Develop REST APIs",
    "Write maintainable code"
  ]
}
```

---

# 📄 Step 2 — Resume Text Extraction

The application checks the file extension.

For PDF files:

```text
PDF → PyPDF → Extracted Text
```

For DOCX files:

```text
DOCX → python-docx → Extracted Text
```

The extracted text is then passed to the AI resume parser.

---

# 🧠 Step 3 — Resume Parsing

The AI analyzes the resume based on its meaning rather than depending only on exact section headings.

For example, the following sections can all be interpreted as work experience:

```text
Experience
Professional Experience
Work History
Employment
Internships
```

The parser extracts:

```text
Name
Email
Phone
Total Experience
Skills
Experience
Education
Projects
Certifications
```

The extracted information is validated using Pydantic models.

---

# 🎯 Step 4 — Candidate Matching

The structured Job Description and structured Resume are passed to the AI matching system.

The system evaluates:

* Matching skills
* Missing important skills
* Experience requirements
* Educational requirements
* Relevant experience
* Overall job relevance

The candidate receives a score between:

```text
0 - 100
```

---

# 📊 Step 5 — Candidate Ranking

After each resume is processed, the candidate name, score, and details are stored.

Candidates are sorted by their score in descending order.

Example:

```text
Candidate          Score
-------------------------
John Doe            92
Rahul Kumar         84
Priya Singh         71
Candidate 4         58
```

This allows recruiters to quickly identify the most relevant candidates.

---

# ▶️ Running the Application

Make sure:

1. `uv` is installed.
2. Dependencies are installed.
3. `.env` contains your Groq API key.
4. Candidate resumes are inside the `resume/` directory.

Run the project using:

```powershell
uv run python miniProject.py
```

If the virtual environment is activated, you can also run:

```powershell
python miniProject.py
```

---

# 📤 Example Output

The application processes each resume and displays its result.

Example:

```text
Processing: candidate1.pdf

Score: 91

{
    "matching_skills": [
        "Java",
        "Spring Boot",
        "REST APIs",
        "MySQL"
    ],
    "missing_important_skills": [
        "React.js"
    ],
    "experience_required": true,
    "verdict": "Strong match for the role."
}
```

The application then continues processing the next resume.

---

# 🔄 Complete Workflow

A typical workflow looks like:

```text
1. Start the application
          ↓
2. Provide Job Description
          ↓
3. AI extracts job requirements
          ↓
4. Application scans resume/
          ↓
5. PDF/DOCX text is extracted
          ↓
6. AI parses each resume
          ↓
7. Resume is compared with Job Description
          ↓
8. Match score is generated
          ↓
9. Matching/missing skills are identified
          ↓
10. Candidates are ranked
```

---

# ⏱️ Processing Time

Each resume requires AI processing for:

```text
Resume Parsing
      +
Job Matching
```

Processing time depends on:

* Number of resumes
* Resume size
* Groq API response time
* API rate limits

The current implementation processes resumes sequentially and includes delays between API requests.

---

# 🔐 Security

This project requires a Groq API key.

Never hard-code your API key in Python source code.

Use:

```env
GROQ_API_KEY=your_api_key
```

inside `.env`.

If an API key is accidentally exposed or committed to Git, **revoke it immediately and generate a new key**.

Do not upload real resumes containing personal information to a public repository.

For testing, use sample or dummy resumes.

---

# ⚠️ Current Limitations

This project is currently a **mini project / prototype**.

Current limitations include:

* AI-generated scores may not always be deterministic.
* Resume parsing depends on the quality of extracted text.
* Scanned/image-only PDFs are not supported because OCR is not implemented.
* The application currently runs through the command line.
* Resumes are processed sequentially.
* Scoring is LLM-based rather than a fixed weighted scoring algorithm.
* There is currently no web dashboard.
* Candidate data is not stored in a database.

---

# 🔮 Future Improvements

Possible future improvements include:

* Build a React-based web interface.
* Add resume upload functionality.
* Add Job Description upload.
* Add configurable scoring weights.
* Add OCR support for scanned resumes.
* Process multiple resumes concurrently.
* Export candidate rankings to CSV/Excel.
* Add a recruiter dashboard.
* Store candidate information in a database.
* Add authentication and user management.
* Add detailed skill-by-skill scoring.
* Add resume improvement suggestions.
* Support multiple Job Descriptions.
* Add candidate filtering by score, experience, and skills.

---

# 🎯 Example Use Case

Imagine a recruiter receives **100 resumes** for a Software Engineer position.

Instead of manually reviewing every resume:

```text
Job Description
       ↓
AI extracts requirements
       ↓
100 Resumes
       ↓
AI parses resumes
       ↓
AI compares candidates
       ↓
Match Scores
       ↓
Candidate Ranking
```

The recruiter can then focus on the highest-scoring candidates first.

---

# 📚 Learning Outcomes

This project demonstrates practical experience with:

* Python
* LLM API integration
* Groq API
* Prompt engineering
* Structured AI output
* Pydantic data validation
* PDF processing
* DOCX processing
* Environment variable management
* Object-oriented programming
* CLI application development
* Resume parsing
* Semantic job matching
* Automated candidate scoring
* Candidate ranking

---

# 👨‍💻 Author

**Ayush Kumar Sinha**

GitHub:
https://github.com/ayush807sinha

---

# ⭐ Support

If you find this project useful or interesting, consider giving the repository a ⭐ on GitHub.
