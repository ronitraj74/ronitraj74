<div align="center">



**A detailed look at what I've built: the problem, the approach, the stack and what I learned.**

[![Portfolio](https://img.shields.io/badge/Portfolio-A371F7?style=for-the-badge&logo=vercel&logoColor=white)](https://rsr-portfolio.vercel.app/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/ronit-raj-114181315)
[![All Repositories](https://img.shields.io/badge/All_Repositories-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Ronit049?tab=repositories)

</div>

---

## 📚 Project Index

| # | Project | Domain | Core Stack |
|:-:|:--|:--|:--|
| 1 | [TestPilot-AI](#1--testpilot-ai--self-improving-test-generation-agent) | Agentic AI · Developer Tools | Python · Groq · PyTest · Streamlit |
| 2 | [Resume Analyzer](#2--resume-analyzer--job-description-matcher) | Generative AI · NLP | Python · LangChain · Groq · Pydantic · Streamlit |
| 3 | [Blinkit Store Placement Prediction](#3--blinkit-store-placement-prediction) | Machine Learning · Geospatial | Python · Scikit-learn · Folium · Pandas |
| 4 | [Find the Perfect Blinkit Location](#4--find-the-perfect-blinkit-location) | Location Intelligence · Web | HTML · CSS · JavaScript · Vercel |
| 5 | [GitHub Developer Analytics](#5--github-developer-analytics) | Data Dashboard · Web | JavaScript · GitHub REST API |
| 6 | [WhisperWall](#6--whisperwall--public-confession-app) | Frontend · UI/UX | HTML · CSS · JavaScript |

---

## 1. 🧪 TestPilot-AI — Self-Improving Test Generation Agent

[![Repo](https://img.shields.io/badge/View_Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Ronit049/TestPilot-AI)
![Stars](https://img.shields.io/github/stars/Ronit049/TestPilot-AI?style=for-the-badge&logo=github&color=a371f7)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Groq](https://img.shields.io/badge/Groq-F55036?style=for-the-badge)
![PyTest](https://img.shields.io/badge/PyTest-0A9EDC?style=for-the-badge&logo=pytest&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)

### Overview
TestPilot-AI is an **agentic AI system that writes, runs and improves unit tests on its own**. A developer pastes Python code into a Streamlit app, and a chain of specialised AI agents analyses it, generates PyTest test cases, executes them, and, when tests fail, diagnoses the failures and rewrites the tests before running them again. It was built as a prototype for a **6-hour Agentic AI hackathon**.

### The Problem
Writing good unit tests is slow and often skipped. Even AI-generated tests are usually "one-shot": if they fail or are badly written, a human has to debug them. I wanted to explore what happens when the AI gets **feedback from real test execution** and uses it to correct itself.

### How It Works
```text
 Developer's Python Code
          │
          ▼
 ┌─────────────────────┐
 │  Code Analyzer Agent │  → understands functions, edge cases, produces a testing plan
 └──────────┬──────────┘
            ▼
 ┌─────────────────────┐
 │ Test Generator Agent │  → writes PyTest code from the plan
 └──────────┬──────────┘
            ▼
 ┌─────────────────────┐
 │    PyTest Runner     │  → executes the generated tests
 └──────────┬──────────┘
        PASS │ FAIL
             ▼
 ┌─────────────────────┐
 │ Failure Analyzer /   │  → reads the error output, rewrites the tests
 │ Test Improver Agent  │  → loops back to the runner
 └─────────────────────┘
```

### Key Features
- **Multi-agent pipeline** with separate agents for analysis, generation and improvement
- **Closed feedback loop**: real PyTest output drives the next round of improvements
- **One-click workflow** in Streamlit: paste code, click *Generate & Run Tests*, see results
- **Fast inference with Groq**, which keeps the generate-run-improve loop responsive enough for a live demo
- **Simple setup** with a virtual environment, a `requirements.txt` and a `.env` file for the API key

### Tech Stack
`Python` · `Groq API` · `PyTest` · `Streamlit` · `Agentic AI` · `Prompt Engineering` · `subprocess`

### What I Learned
Designing agent roles with clear responsibilities, passing structured context between agents, building self-correction loops, and the safety implications of executing generated code. The README notes that the local subprocess runner is a controlled prototype and **should not be used to run untrusted code in production**.

---

## 2. 📄 Resume Analyzer & Job Description Matcher

[![Repo](https://img.shields.io/badge/View_Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Ronit049/Resume-Analyzer)
![Stars](https://img.shields.io/github/stars/Ronit049/Resume-Analyzer?style=for-the-badge&logo=github&color=a371f7)
![Forks](https://img.shields.io/github/forks/Ronit049/Resume-Analyzer?style=for-the-badge&logo=github&color=555)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)

### Overview
Resume Analyzer is an **AI-powered web application** that reads a resume PDF, converts it into clean structured data, and compares it against a job description to produce an **ATS match score** with recruiter-style insights. Instead of reading resumes manually, it uses Large Language Models through **Groq and LangChain** to extract and evaluate everything automatically.

### The Problem
Recruiters and candidates both lose time on unstructured resumes. Candidates rarely know why an Applicant Tracking System (ATS) ranked them low, and recruiters have to scan dozens of PDFs for the same information.

### What It Extracts and Produces
- **Candidate details:** name, email, phone number
- **Education and experience**
- **Technical skills**
- **Job-fit analysis:** ATS match score, matched skills, **missing skills**, strengths, gaps and a short **recruiter summary**
- **Exportable results:** the parsed resume and the match report can both be downloaded as JSON

### How It Works
```text
 Resume PDF → PyPDFLoader → plain text
      → LangChain PromptTemplate → Groq Llama 3.3
      → structured Resume object (Pydantic)
      + Job Description text
      → AI Resume Matcher → JDMatchResult (structured)
      → Streamlit dashboard + JSON downloads
```
The model returns **validated structured output through function calling and Pydantic models**, not free text, so results are predictable and can be reused by other code.

### Project Structure
`models.py` (Pydantic schemas) · `prompts.py` (prompt templates) · `parser.py` (resume extraction) · `matcher.py` (JD comparison) · `main.py` (Streamlit app)

### Tech Stack
`Python` · `LangChain` · `Groq (Llama 3.3)` · `Pydantic` · `PyPDF / PyPDFLoader` · `Streamlit` · `python-dotenv`

### What I Learned
Prompt engineering for reliable extraction, structured output and function calling, chaining LLM steps with LangChain, validating model output with Pydantic, and shipping an AI feature as a usable web interface.

### Planned Improvements
DOCX resume support · OCR for scanned PDFs · multi-resume ranking · resume improvement suggestions · skill-gap visualisation · ATS compatibility analysis

---

## 3. 🛵 Blinkit Store Placement Prediction

[![Repo](https://img.shields.io/badge/View_Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Ronit049/BLINKIT-store-placement-prediction-project-)
![Stars](https://img.shields.io/github/stars/Ronit049/BLINKIT-store-placement-prediction-project-?style=for-the-badge&logo=github&color=a371f7)
![Forks](https://img.shields.io/github/forks/Ronit049/BLINKIT-store-placement-prediction-project-?style=for-the-badge&logo=github&color=555)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)

### Overview
A **machine learning and business-intelligence project** that recommends the best locations for new quick-commerce stores. It combines **clustering, weighted scoring and geospatial visualisation** to turn raw location and demographic data into an interactive map of high-potential zones.

### The Problem Statement
> *"Where should we open our next store to maximise visibility, reach and profitability?"*

Choosing a store location is a high-stakes business decision, and many businesses make it without actionable data. This project makes the decision visual and data-driven.

### Approach
1. **Load and clean** location and demographic data (area, latitude/longitude, population, competitor count, average income, foot traffic)
2. **Score every zone** with a weighted formula over factors such as population density, competition and traffic
3. **Cluster similar regions** using **KMeans** to segment the city into meaningful groups
4. **Visualise results** on an interactive map with colour-coded zones (good, average, bad)
5. **Highlight the top-N zones** for opening a store, with high-potential areas in green

### Key Features
- 📍 Interactive map showing store potential by zone
- 📊 Transparent, weighted scoring system
- 🤖 KMeans clustering for zone segmentation
- 📈 Prediction of the top *n* best zones
- 💻 Optional Streamlit interface
- 🖼️ Screenshots of the top predicted zone and the delivery coverage map

### Tech Stack
`Python` · `Pandas` · `NumPy` · `Scikit-learn (KMeans)` · `Folium` · `Matplotlib` · `Plotly` · `Streamlit`

### Planned Enhancements
Real-time data through the Google Places API · support for other business types (café, pharmacy) · ROI prediction from rent and expected revenue · animated heatmaps over time

---

## 4. 📍 Find the Perfect Blinkit Location

[![Repo](https://img.shields.io/badge/View_Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Ronit049/Find-the-Perfect-Blinkit-Location)
[![Live Demo](https://img.shields.io/badge/Live_Demo-22c55e?style=for-the-badge&logo=vercel&logoColor=white)](https://find-the-perfect-blinkit-location.vercel.app/)
![Stars](https://img.shields.io/github/stars/Ronit049/Find-the-Perfect-Blinkit-Location?style=for-the-badge&logo=github&color=a371f7)
![HTML](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)

### Overview
A **web-based location-intelligence tool** that helps identify the most suitable area for opening a new Blinkit store or delivery hub. Where project 3 focuses on the machine-learning model, this project turns the idea into a **live, map-based website** that anyone can open and explore.

### Why It Matters
In quick-commerce, store placement directly affects **delivery speed, customer reach and operating efficiency**. This project shows how map visualisation and location analysis can support smarter expansion decisions.

### Factors Considered
Customer demand · population density · nearby residential areas · accessibility and road connectivity · competitor presence · delivery coverage · delivery time · business growth potential

### Key Features
- Interactive, map-based website that is simple to use
- Visual analysis of suitable store-placement areas
- Top predicted zone view and delivery-coverage map
- Clear presentation of how demand, accessibility and coverage interact
- Deployed on **Vercel**, so it works straight from a link

### Tech Stack
`HTML` · `CSS` · `JavaScript` · `Map visualisation` · `Vercel`

---

## 5. 📊 GitHub Developer Analytics

[![Repo](https://img.shields.io/badge/View_Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Ronit049/GitHub-Developer-Analytics)
![Stars](https://img.shields.io/github/stars/Ronit049/GitHub-Developer-Analytics?style=for-the-badge&logo=github&color=a371f7)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![GitHub API](https://img.shields.io/badge/GitHub_REST_API-181717?style=for-the-badge&logo=github&logoColor=white)
![No Build](https://img.shields.io/badge/Zero_Build_Step-C7F255?style=for-the-badge&labelColor=101315)

### Overview
A fast, **privacy-conscious dashboard** for exploring the public footprint of any GitHub developer. Enter a username and see profile signals, repository performance, language preferences and recent activity in a single view, turning raw GitHub data into **actionable insights**.

### Key Features
- **Profile overview:** name, avatar, followers, following, public repository count
- **Repository signals:** total stars and forks across public repositories
- **Language breakdown:** primary languages ranked by how many repositories use them
- **Activity pulse:** a 0–100 score based on repositories updated in the last 90 days plus recent public events
- **Popular projects:** the five repositories with the strongest star and fork signals
- **Thoughtful states:** clear loading, missing-profile and API rate-limit feedback
- **Responsive design** for desktop and mobile

### How It Works
```text
 GitHub username
      │
      ▼
 GitHub REST API
   ├── /users/{username}
   ├── /users/{username}/repos
   └── /users/{username}/events/public
      │
      ▼
 Profile · repository · language · activity analytics (rendered in the browser)
```

### Engineering Notes
- Runs **entirely in the browser** with no backend, no sign-in and no access token, and reads only public data
- **Dependency-free static site** (`index.html`, `styles.css`, `app.js`), deployable to GitHub Pages, Netlify or Vercel
- Documents its metric formulas and handles the 100-repositories-per-request API limit transparently
- Recommends a server-side proxy with a securely stored token for heavier production traffic

### Tech Stack
`JavaScript` · `HTML5` · `CSS3` · `GitHub REST API`

---

## 6. 🕯️ WhisperWall — Public Confession App

[![Repo](https://img.shields.io/badge/View_Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Ronit049/-WhisperWall)
[![Live Demo](https://img.shields.io/badge/Live_Demo-22c55e?style=for-the-badge&logo=vercel&logoColor=white)](https://whisper-wall-blue.vercel.app/)
![Stars](https://img.shields.io/github/stars/Ronit049/-WhisperWall?style=for-the-badge&logo=github&color=7c3aed)
![Forks](https://img.shields.io/github/forks/Ronit049/-WhisperWall?style=for-the-badge&logo=github&color=8b5cf6)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)

### Overview
WhisperWall is a **beautiful, dark-themed public confession website** with a login and OTP verification flow. Visitors can share thoughts anonymously, browse a masonry-style wall of confessions, like, search and sort them, all in a polished, modern interface.

### Login Flow
1. **Step 1:** enter name and email
2. **Step 2:** 6-digit OTP with auto-advance between boxes, paste support and a 60-second resend countdown
3. **Step 3:** welcome screen, then redirect to the wall

The session persists across refreshes, and the wall page is **protected by an auth guard** that redirects unverified visitors back to login.

### Confession Wall Features
- Post confessions with an **optional title and mood emoji**
- **1,000-character limit** with a live counter
- Responsive **masonry-style card grid**
- Click a card to open a **lightbox** with the full text
- **Like** system (one like per user)
- **Share** through the Web Share API, with a clipboard fallback
- **Search** by title or content, and **sort** by newest, oldest or most liked
- **Logout** clears the session

### Architecture
Clean separation of concerns: `index.html` and `wall.html` for pages, `style.css` / `login.css` / `wall.css` for global and page-specific styling with CSS variables, and `auth.js` / `wall.js` for authentication and wall logic.

> **Note:** this is a front-end showcase. With no backend, the OTP is a demo flow, and data is kept in the browser.

### Tech Stack
`HTML5` · `CSS3 (variables, animations)` · `Vanilla JavaScript` · `Web Share API` · `localStorage` · `Vercel`

---

<div align="center">

### 🗂️ There's more

I have **100+ repositories** covering coursework, practice, experiments and internship work.

[![Browse All Repositories](https://img.shields.io/badge/Browse_All_Repositories-a371f7?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Ronit049?tab=repositories)

*Learning today, building tomorrow.*

<img src="https://capsule-render.vercel.app/api?type=waving&height=90&section=footer&color=0:a371f7,100:000000" width="100%" alt=""/>

</div>
