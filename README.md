# final-year-project
README: AI-Based Resume Matching and Job Description Analysis System Using ServiceNow
Project Overview
This project is an AI-assisted resume matching and job description analysis system built inside the ServiceNow platform. The system allows recruiters to upload one Job Description and multiple candidate resumes in PDF format, compare each resume against the job description using AI, extract a fit percentage, classify candidates, and store the results in ServiceNow custom tables for audit history.
The system improves traditional Applicant Tracking System screening by moving beyond simple keyword matching. Instead of only checking whether exact words appear in a resume, the system uses Large Language Model based semantic evaluation to understand skills, experience, transferable knowledge, and role suitability.
Key Features
The system supports job description PDF upload, multiple resume PDF upload, PDF preview, AI model selection, AI-based semantic comparison, fit percentage extraction, candidate classification, database storage, and history review.
Recruiters can select either ChatGPT-style evaluation or DeepSeek-style reasoning. The frontend provides file upload sections, model selection, result cards, preview tabs, and a progress overlay. The backend handles attachment storage, PDF text extraction, AI API communication, Regex score extraction, candidate classification, and ServiceNow database insertion.
Technologies Used
ComponentTechnologyFrontendHTML5, CSS3, AngularJSPlatformServiceNow Service PortalBackendServiceNow Server-side JavaScriptFile StorageGlideSysAttachmentDatabase OperationsGlideRecordExternal API CallsRESTMessageV2PDF Text ExtractionGemini APIAI Resume MatchingOpenAI Chat Completions APIScore ExtractionJavaScript RegexUI FeedbackProgress Ring Animation
System Architecture
The system follows a multi-tier architecture.
The Presentation Tier is built using a ServiceNow Service Portal widget. It provides the recruiter-facing interface, including upload areas, AI model selector, preview tabs, progress overlay, result cards, and View History button.
The Client-Side Processing Tier is built using AngularJS. It handles file selection, PDF validation, preview generation, Base64 conversion, progress animation, and communication with the ServiceNow backend.
The Backend Orchestration Tier is built using ServiceNow server-side JavaScript. It saves uploaded files as attachments, extracts text, builds AI prompts, sends requests to external APIs, parses AI responses, classifies candidates, and stores records.
The External AI Tier uses Gemini API for PDF text extraction and OpenAI Chat Completions API for resume-to-job-description semantic evaluation.
The Data Persistence Tier uses ServiceNow custom tables to store job description records and candidate resume evaluation records.
Main Workflow


Recruiter opens the ServiceNow portal.


Recruiter uploads one Job Description PDF.


Recruiter uploads one or more Resume PDFs.


Recruiter selects the AI model.


Frontend validates uploaded PDFs.


Frontend creates PDF previews.


Frontend converts uploaded files into Base64 format.


Frontend sends job description, resumes, and selected model to the backend.


Backend saves files as ServiceNow attachments.


Backend extracts text from the job description and resumes.


Backend creates structured AI prompts.


Backend sends prompts to the external AI service.


AI returns reasoning and fit percentage.


Backend extracts fit percentage using Regex.


Backend classifies the candidate.


Backend stores the results in ServiceNow custom tables.


Frontend displays candidate scores and status.


Recruiter reviews the results and may open View History.


Candidate Classification Logic
The system classifies candidates using the extracted fit percentage.
Fit PercentageCandidate StatusMeaning70–100EligibleCandidate strongly matches the job description.50–69Need ReviewCandidate has partial alignment or transferable skills.0–49Not EligibleCandidate has weak alignment with the role.
The classification helps recruiters quickly identify strong candidates, borderline candidates, and weak matches. The Need Review category supports human judgement and prevents strict automatic rejection of borderline candidates.
AI Evaluation Method
The system uses AI to compare extracted resume text with extracted job description text. The prompt instructs the AI model to act as an expert recruiter and evaluate the candidate based on skills, experience, education, project relevance, transferable knowledge, and missing requirements.
The AI is instructed to return a fit percentage using a fixed format:
FIT PERCENTAGE: <number>
This fixed marker allows the backend to extract the numerical value reliably using Regex.
Regex Score Extraction
The backend extracts the fit percentage using this Regex pattern:
/FIT PERCENTAGE:\s*([0-9]{1,3}(?:.[0-9]+)?)/i
This pattern can extract both integer and decimal values, such as:
FIT PERCENTAGE: 88
FIT PERCENTAGE: 74.5
Fit Percentage: 63
If no valid fit percentage is found, the system returns 0 as the default score or handles the parsing failure gracefully.
ServiceNow Custom Tables
The project uses two main custom tables.
Job Description Table
Suggested table name:
u_job_description
Main fields:
FieldPurposeu_job_titleStores extracted job titleu_required_skillsStores required skillsu_experienceStores expected experienceu_education_levelStores required education level
Resume Evaluation Table
Suggested table name:
u_resumes
Main fields:
FieldPurposeu_job_descriptionReference to parent job descriptionu_resume_attachmentStores uploaded resume attachment referenceu_fit_percentageStores AI-generated fit percentageu_statusStores candidate classification
Required System Properties
Before running the project, configure these ServiceNow system properties:
openai.api.key
google.gemini.api.key
google.gemini.model
PropertyDescriptionopenai.api.keyAPI key for OpenAI Chat Completionsgoogle.gemini.api.keyAPI key for Gemini PDF text extractiongoogle.gemini.modelGemini model name, for example gemini-3-flash-preview
Installation and Setup
Step 1: Create ServiceNow Custom Tables
Create the following custom tables:
u_job_description
u_resumes
Add the required fields listed in the database section.
Step 2: Create a Service Portal Widget
Create a new ServiceNow Service Portal widget. The widget should include:
HTML template
CSS
Client Script
Server Script
Link Function
Step 3: Add the HTML Code
Paste the HTML code into the widget HTML template section.
Step 4: Add the CSS Code
Paste the CSS styling into the widget CSS section.
Step 5: Add the Client Script
Paste the AngularJS client script into the widget Client Script section.
Step 6: Add the Server Script
Paste the backend ServiceNow server script into the widget Server Script section.
Step 7: Configure API Keys
Add the OpenAI and Gemini API keys as ServiceNow system properties.
Step 8: Add the Widget to a Portal Page
Place the widget on a ServiceNow portal page, for example:
/sp1
Step 9: Configure the View History Page
The project includes a View History link:
/sp1?id=job_history_details
Create or configure this page to display previous job description and candidate evaluation records.
How to Use


Open the ServiceNow portal page containing the widget.


Upload one Job Description PDF.


Upload one or more Resume PDFs.


Select the AI model: ChatGPT or DeepSeek.


Click Analyze.


Wait for the progress indicator to complete.


Review candidate fit percentages and status.


Open View History to review previous screening records.


Example Output
CandidateFit PercentageStatusCandidate 188%EligibleCandidate 264%Need ReviewCandidate 342%Not Eligible
Error Handling
The system includes error handling for missing job descriptions, missing resumes, invalid file types, missing API keys, PDF extraction failure, OpenAI API failure, Gemini API failure, invalid AI responses, Regex parsing failure, and database insertion errors.
If an error occurs, the backend logs the issue and returns a controlled response. This prevents the full workflow from crashing if one resume fails during batch processing.
Project Limitations
The system depends on external AI APIs. Free-tier APIs may cause latency, rate limits, or timeout issues. Scanned PDFs may not extract correctly without OCR. AI responses may vary despite prompt engineering. Regex parsing depends on the AI including the FIT PERCENTAGE marker. The system is a decision-support tool and should not make final hiring decisions automatically. Human recruiter review is still required.
Future Improvements
Future versions of the system could include OCR support for scanned resumes, structured JSON output instead of Regex parsing, recruiter override tracking, role-based access control, automatic interview scheduling, analytics dashboards, bias monitoring, fairness testing, local or private LLM deployment, and asynchronous background processing using ServiceNow Events or queues.
Security Notes
API keys should never be placed in frontend code. All external API communication should happen from the ServiceNow backend. Candidate resumes may contain personal information and should be protected using ServiceNow access controls. Only authorized recruiters and administrators should access stored evaluation records. For enterprise deployment, organizations should review data privacy and AI usage policies before processing real candidate data.
Project Author
Sosan Ali Butt
Department of Computer Science
Ulster University
Project Title: AI-Based Resume Matching and Job Description Analysis System Using ServiceNow
License
This project is developed for academic purposes as part of a Final Year Project or MSc project. Further use, deployment, or modification should follow the academic and organizational policies of the institution or company using it.
