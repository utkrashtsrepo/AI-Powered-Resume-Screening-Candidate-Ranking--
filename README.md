 Resume Screening Project

An automated, LLM-powered resume parsing and screening system built with **Python**, **Groq LLM API**, and **Pydantic**. It extracts text from candidate resumes in multiple formats (`.pdf`, `.docx`), parses job descriptions into structured requirements, evaluates each candidate using semantic matching, and ranks them with match percentages and recruiter insights.

---

## 📁 Repository Overview

```text
week1/day5/
├── resumes/                                       # Folder containing candidate resumes
│   ├── abhay resume new - Abhay Singh.pdf
│   ├── anshit verma resume word - Anshit Verma.docx
│   ├── Ashish Raj 24PCS007 (1) - Ashish Raj.pdf
│   └── Resume for Freshers - Priyanshu Singh.docx
├── .env.example                                   # Sample environment configuration
├── pyproject.toml                                 # Dependencies and project metadata
├── resume_parser.py                               # Core resume parsing & evaluation script
├── main.py                                        # Default entry point
└── README.md                                      # Documentation
```

---

## ⚙️ How It Works

The workflow in `resume_parser.py` executes in five stages:

1. **Job Description Extraction**:
   - Takes a job description (e.g., Amazon SDE-I).
   - Prompts Groq LLM with a Pydantic `JobD` schema (`role`, `required_skills`, `preferred_skills`, `minimum_experience`, `education_requirements`, `responsibilities`).
   - Parses the JSON output into a typed `JobD` object.

2. **Resume Document Ingestion**:
   - Reads `.pdf` documents page by page using `pypdf`.
   - Reads `.docx` documents (paragraphs and table cells) using `python-docx`.

3. **Semantic Resume Parsing**:
   - Passes extracted text to Groq LLM using JSON mode.
   - Extracts structured candidate records into `Resume` schema (`name`, `email`, `phone`, `total_experience_years`, `skills`, `experiences`, `education`, `projects`, `certifications`).

4. **Candidate Matching & Scoring**:
   - Compares the structured `JobD` and `Resume` models.
   - Computes an overall match score (0-100%) and generates structured details:
     - Candidate name
     - Matching skills
     - Missing important skills
     - Experience qualification check
     - Short final verdict

5. **Sorting & Ranking**:
   - Sorts all candidates descending by score.
   - Displays the **Top 2 Candidates** and **Lowest 2 Candidates** in the console.

---

## 🚀 Setup & Execution

### 1. Prerequisites
- Python `>= 3.11`
- A [Groq API key](https://console.groq.com/keys)

### 2. Install Dependencies

You can install the required packages using `pip`:
```powershell
pip install groq pydantic pypdf python-docx python-dotenv
```

Or using `uv` if configured:
```powershell
uv sync
```

### 3. Configure API Key
Create a `.env` file in this directory (`week1/day5/.env`):
```ini
GROQ_API_KEY=gsk_your_groq_api_key_here
```

### 4. Run the Resume Screener
Make sure your resume files are inside the `resumes/` folder, then run:
```powershell
python resume_parser.py
```

---

## 🧪 Verification & Status Check

- **Document Ingestion**: Verified working for both PDF and DOCX files:
  - `abhay resume new - Abhay Singh.pdf` (1,845 characters extracted)
  - `anshit verma resume word - Anshit Verma.docx` (790 characters extracted)
  - `Ashish Raj 24PCS007 (1) - Ashish Raj.pdf` (3,332 characters extracted)
  - `Resume for Freshers - Priyanshu Singh.docx` (2,702 characters extracted)
- **Dependencies**: All required packages (`groq`, `pydantic`, `pypdf`, `python-docx`, `python-dotenv`) are installed and functional.
- **Pydantic Schemas**: `JobD`, `Resume`, `Experience`, and `MatchResult` are properly defined and validated against Pydantic v2.
- **LLM API Calls**: Requires a valid `GROQ_API_KEY` set in `.env` to execute live chat completions.

