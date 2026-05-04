# AI-Based Resume Matching and Job Description Analysis System Using ServiceNow

## Overview

This project is an AI-assisted resume matching and job description analysis system developed using the ServiceNow platform. The system allows recruiters to upload a Job Description and multiple candidate resumes in PDF format, compare each resume against the job description using AI, extract a fit percentage, classify candidates, and store the evaluation results in ServiceNow custom tables.

The system improves traditional Applicant Tracking System screening by using semantic AI evaluation instead of simple keyword matching.

---

## Project Title

**AI-Based Resume Matching and Job Description Analysis System Using ServiceNow**

---

## Author

**Sosan Ali Butt**  
Department of Computer Science  
Ulster University  

---

## Main Features

- Upload one Job Description PDF
- Upload multiple candidate Resume PDFs
- Validate uploaded PDF files
- Preview uploaded job description and resumes
- Select AI evaluation mode
- Compare resumes with job description using AI
- Generate AI-based reasoning
- Extract fit percentage using Regex
- Classify candidates as:
  - Eligible
  - Need Review
  - Not Eligible
- Store results in ServiceNow custom tables
- View previous analysis history
- Handle API latency using progress indicator
- Maintain audit records for recruitment decisions

---

## Technologies Used

| Component | Technology |
|---|---|
| Frontend | HTML5, CSS3, AngularJS |
| Platform | ServiceNow Service Portal |
| Backend | ServiceNow Server-side JavaScript |
| File Storage | GlideSysAttachment |
| Database Operations | GlideRecord |
| External API Calls | RESTMessageV2 |
| PDF Text Extraction | Gemini API |
| AI Resume Matching | OpenAI Chat Completions API |
| Response Parsing | JavaScript Regex |
| UI Feedback | Progress Ring / Loading Overlay |

---

## System Architecture

The project follows a multi-tier architecture:

```text
Recruiter
   |
   v
ServiceNow Service Portal
   |
   v
AngularJS Client Script
   |
   v
ServiceNow Server Script
   |
   |-- GlideSysAttachment
   |-- PDF Text Extraction API
   |-- RESTMessageV2
   |-- OpenAI / DeepSeek-style AI Evaluation
   |-- Regex Score Extraction
   |-- Candidate Classification
   |
   v
ServiceNow Custom Tables
   |
   v
View History / Audit Records

Candidate Classification Logic
| Fit Percentage | Candidate Status | Meaning                                        |
| -------------- | ---------------- | ---------------------------------------------- |
| 70–100         | Eligible         | Candidate strongly matches the job description |
| 50–69          | Need Review      | Candidate has partial or transferable skills   |
| 0–49           | Not Eligible     | Candidate has weak alignment with the role     |

Classification Formula
Candidate Status =
    Eligible       if Score >= 70
    Need Review    if 50 <= Score < 70
    Not Eligible   if Score < 50
Regex Pattern Used

The backend extracts the AI-generated fit percentage using Regex.
/FIT PERCENTAGE:\s*([0-9]{1,3}(?:\.[0-9]+)?)/i
This pattern extracts scores such as:
FIT PERCENTAGE: 88
FIT PERCENTAGE: 74.5
Fit Percentage: 63
MAIN WORKFLOW
1. Recruiter opens the ServiceNow portal
2. Recruiter uploads one Job Description PDF
3. Recruiter uploads multiple Resume PDFs
4. Recruiter selects AI model
5. Frontend validates files
6. Frontend creates PDF previews
7. Frontend converts files to Base64
8. Backend saves files as ServiceNow attachments
9. Backend extracts text from PDFs
10. Backend builds AI prompt
11. Backend sends prompt to external AI API
12. AI returns reasoning and fit percentage
13. Backend extracts score using Regex
14. Backend classifies candidate
15. Backend stores results in custom tables
16. Frontend displays candidate results
17. Recruiter reviews results
18. Recruiter can view history records

ServiceNow Custom Tables
Job Description Table

Suggested table name:
u_job_description
Suggested fields:
| Field Name        | Purpose                       |
| ----------------- | ----------------------------- |
| u_job_title       | Stores extracted job title    |
| u_required_skills | Stores required skills        |
| u_experience      | Stores experience requirement |
| u_education_level | Stores education requirement  |
| u_created_on      | Stores creation timestamp     |

Candidate Resume Evaluation Table

Suggested table name:

u_resumes

Suggested fields:
Field NamePurposeu_job_descriptionReference to parent job descriptionu_resume_attachmentStores resume attachment referenceu_fit_percentageStores extracted AI fit scoreu_statusStores candidate statusu_ai_reasoningStores AI-generated reasoningu_model_usedStores selected AI modelu_processed_onStores processing timestamp

Required System Properties
Configure the following system properties in ServiceNow:
openai.api.keygoogle.gemini.api.keygoogle.gemini.model
Example:
openai.api.key = YOUR_OPENAI_API_KEYgoogle.gemini.api.key = YOUR_GEMINI_API_KEYgoogle.gemini.model = gemini-3-flash-preview

Installation Steps
Step 1: Create ServiceNow Custom Tables
Create the following custom tables:
u_job_descriptionu_resumes
Add the required fields listed above.

Step 2: Create a Service Portal Widget
Create a new widget in ServiceNow Service Portal.
The widget should contain:
HTML TemplateCSSClient ScriptServer Script

Step 3: Add Frontend Code
Add the HTML structure for:
Job Description uploadResume uploadModel selectorAnalyze buttonView History buttonPDF previewProgress overlayResult cards

Step 4: Add CSS Styling
Add styles for:
Upload containersButtonsPDF previewProgress ringCandidate cardsEligible statusNeed Review statusNot Eligible status

Step 5: Add AngularJS Client Script
The client script should handle:
File selectionPDF validationPDF previewBase64 conversionModel selectionProgress indicatorServer communicationResult display

Step 6: Add Server Script
The server script should handle:
Receiving uploaded filesSaving attachmentsExtracting PDF textBuilding AI promptsCalling external APIsParsing AI responseExtracting fit percentageClassifying candidatesSaving records using GlideRecordReturning results to frontend

Step 7: Configure API Keys
Store API keys securely in ServiceNow system properties.
Do not place API keys in frontend code.

Step 8: Add Widget to Portal Page
Add the widget to a ServiceNow portal page, for example:
/sp1

Step 9: Configure View History Page
Create or configure the history page:
/sp1?id=job_history_details
This page should display previous job descriptions and related candidate evaluations.

Example AI Prompt Structure
You are an expert HR recruiter.Compare the following candidate resume against the job description.Evaluate:- Technical skills- Experience- Education- Project relevance- Transferable knowledge- Missing requirements- Overall suitabilityDo not rely only on exact keyword matching.Return your reasoning and end your response with:FIT PERCENTAGE: [number]

Example Output
CandidateFit PercentageStatusCandidate A88%EligibleCandidate B64%Need ReviewCandidate C42%Not Eligible

Error Handling
The system handles the following errors:
Missing job descriptionMissing resume filesInvalid file typePDF extraction failureAPI timeoutAPI rate limitInvalid AI responseRegex parsing failureDatabase insert error
If an error occurs, the system returns a controlled message instead of crashing the full workflow.

Security Considerations


API keys should be stored securely in ServiceNow system properties.


API keys must not be exposed in frontend JavaScript.


Resume data should only be accessible to authorized HR users.


Candidate information should be protected using ServiceNow access controls.


External API usage should follow data privacy policies.


Human recruiters should make final hiring decisions.



Limitations


The system depends on external AI APIs.


Free-tier APIs may cause latency or rate-limit errors.


Scanned PDFs may require OCR support.


AI responses may vary.


Regex parsing depends on the AI returning the expected score marker.


The system was tested using sample data.


It is a decision-support tool, not an autonomous hiring system.



Future Improvements
Possible future improvements include:
OCR support for scanned PDFsJSON-based AI response formatLocal or private LLM deploymentAsynchronous background processingRecruiter override trackingInterview scheduling workflowAnalytics dashboardBias monitoringFairness testingRole-based access controlEnterprise-scale testing

Example Folder Structure
AI-Resume-Matching-ServiceNow/│├── README.md├── docs/│   ├── final_report.docx│   ├── ieee_report.tex│   └── diagrams/│├── servicenow-widget/│   ├── html-template.html│   ├── style.css│   ├── client-script.js│   └── server-script.js│├── database/│   ├── job-description-table-fields.md│   └── resume-evaluation-table-fields.md│└── screenshots/    ├── upload-interface.png    ├── system-architecture.png    ├── execution-flowchart.png    └── results-page.png

Project Status
Status: Completed for academic submissionPlatform: ServiceNowPurpose: MSc / Final Year Project

License
This project was developed for academic purposes. Any future use, modification, or deployment should follow the policies of the university or organization using the system.

Contact
Author: Sosan Ali ButtUniversity: Ulster UniversityDepartment: Computer ScienceProject: AI-Based Resume Matching and Job Description Analysis System Using ServiceNow

