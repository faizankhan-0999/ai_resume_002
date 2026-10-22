ResumeMatch AI
A Streamlit dashboard that compares a PDF resume with a job description using Google's Gemini API. It reports an estimated text-evidence match, matched skills, requirements not evidenced in the resume, strengths, resume improvements, keywords, alignment notes, and interview-preparation topics.
Features
- Premium dark dashboard with match gauge and skill badges.
- PDF extraction and 5 MB upload limit; scanned or text-empty PDFs receive a helpful OCR notice.
- Structured JSON response with validation and friendly error handling.
- Optional in-memory PDF report download.
- No global caching or permanent resume storage by this app.
- Unit tests for parsing, scoring, validation, missing credentials and malformed model responses.
Tech stack
Python 3.11/3.12, Streamlit, official google-genai SDK, Gemini 2.5 Flash, pypdf, Plotly, ReportLab, pytest.
Project structure
app.py
requirements.txt
README.md
.gitignore
.streamlit/config.toml
.streamlit/secrets.toml.example
src/__init__.py
src/resume_parser.py
src/gemini_service.py
src/scoring.py
src/report_generator.py
assets/logo.svg
tests/test_resume_parser.py
tests/test_scoring.py
tests/test_validation.py
Local setup
1. Install Python 3.11 or 3.12.
2. Open a terminal in the project folder and create a virtual environment:
   - Windows: py -m venv .venv then .venv\\Scripts\\activate
   - macOS/Linux: python3 -m venv .venv then source .venv/bin/activate
3. Install dependencies: python -m pip install -r requirements.txt.
4. Obtain a Gemini API key from Google AI Studio. Do not paste it into source code or share it with anyone.
5. Set the key locally using one of these options:
   - Environment variable (PowerShell): $env:GEMINI_API_KEY="your-key"
   - Environment variable (macOS/Linux): export GEMINI_API_KEY="your-key"
   - Or copy .streamlit/secrets.toml.example to .streamlit/secrets.toml and replace the placeholder with your own key. This real secrets file is ignored by Git.
6. Run: streamlit run app.py.
Run tests
python -m pytest -q
Tests mock the Gemini client; they do not make paid or live API calls. To test real API access, run the app with your own key and analyze a sample resume you have permission to share.
Upload to GitHub
1. Sign in to GitHub and create a repository named ai-resume-job-matcher (Public is easiest for Community Cloud; use Private if you prefer, subject to deployment access).
2. Do not upload real resumes, .env, or .streamlit/secrets.toml. .gitignore excludes these common secret files.
3. From this folder, run:git init
   git add .
   git status
   git commit -m "Build ResumeMatch AI dashboard"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/ai-resume-job-matcher.git
   git push -u origin main
4. Before git commit, inspect git status and confirm no credentials or personal resume files are staged.
Deploy to Streamlit Community Cloud
1. Push the project to GitHub.
2. Open https://share.streamlit.io/ and choose Create app.
3. Select repository YOUR_USERNAME/ai-resume-job-matcher, branch main, and entrypoint app.py.
4. In Advanced settings → Secrets, add:GEMINI_API_KEY = "YOUR_REAL_KEY"
5. Choose Python 3.12 if asked, save and deploy. Streamlit installs requirements.txt automatically.
6. Open the deployed app and test with a resume you have permission to process. New commits to the configured branch normally trigger redeployment.
Troubleshooting
- API key missing/authentication: check the spelling GEMINI_API_KEY; locally use environment variables or .streamlit/secrets.toml; on Cloud use app Secrets. Never commit the real key.
- Quota or rate limit: wait and retry; check Google AI Studio/API project quota and billing settings.
- Scanned PDF: text extraction cannot read image-only pages. Run OCR with a trusted tool before upload.
- Dependency build error: check Streamlit Cloud logs and confirm the selected Python version is compatible with requirements.
- Invalid PDF: re-export a normal, unlocked PDF with selectable text.
Privacy, security and limitations
- After explicit consent, extracted resume text and the job description are sent to Google's Gemini API. Review Google's applicable terms and data controls before use. This app does not intentionally save resumes or results to a database, but hosting providers and API providers may process operational data under their own policies.
- Do not upload sensitive documents unless you are comfortable sending their text to the API provider. The app truncates prompt inputs to defined limits and does not cache resume analyses globally.
- The score is an AI estimate, not a certified ATS score, hiring decision, or guarantee of an interview. It may be inaccurate or omit context. Skills listed as not evidenced may simply be absent from the resume wording.
- Never use this score as the sole basis for employment decisions. Review all model suggestions manually; the model must not assess sensitive or protected traits.
- The app has no authentication, per-user account system, or robust distributed rate limiter. For public production use, add appropriate abuse protection, monitoring and privacy review before wider rollout.
License
No license is included. Add a license file only after choosing terms that suit your project.
