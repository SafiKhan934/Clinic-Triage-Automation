# Clinic Triage Automation

An AI-powered patient intake and administrative triage workflow built with **n8n, Google Gemini, Gmail, and Google Sheets**.

The system receives patient submissions, uses AI to classify them, validates the AI-generated output, and routes submissions according to their urgency for administrative follow-up.

> **Important:** This project is designed for administrative triage support only. It does not provide medical diagnoses, treatment recommendations, or replace professional clinical assessment.

## Project Overview

Clinics may receive patient submissions containing symptoms, appointment requests, and general questions. Manually reviewing and organizing every submission can take time and may delay the identification of submissions that require staff attention.

This automation demonstrates how AI and workflow automation can help organize incoming patient information and notify clinic staff when a submission requires attention.

## Workflow Process

1. **Patient Intake:** Receives patient information through an n8n Webhook (POST request).
2. **AI Classification:** Google Gemini analyzes the submitted information and assigns a category, urgency score, and short factual summary.
3. **Output Validation:** A JavaScript Code node parses and validates the AI response, checking the category, urgency score, and summary.
4. **Alert Decision:** An IF node determines whether the submission requires an immediate email notification.
5. **Email Notification:** Gmail sends an alert to the configured clinic recipient for submissions meeting the alert conditions.
6. **Record Management:** Patient information and classification results are prepared and saved to Google Sheets.

## Workflow Demonstration

### 1. Complete Workflow Overview

![Workflow Overview](screenshots/01-Workflow-Overview.png)

### 2. Successful Execution

![Successful Execution](screenshots/02-Successful-Execution.png)

### 3. AI Patient Classification

![AI Classification Node](screenshots/08-AI-Classification-Node.png)

### 4. Patient Record Preparation

![Prepare Patient Record](screenshots/09-Prepare-Patient-Record.png)

### 5. Emergency Alert Notification

![Emergency Alert - Top](screenshots/04-Emergency-Alert-Top.png)

![Emergency Alert - Bottom](screenshots/05-Emergency-Alert-Bottom.png)

### 6. Needs Review Notification

![Needs Review Alert - Top](screenshots/06-Needs-Review-Alert-Top.png)

![Needs Review Alert - Bottom](screenshots/07-Needs-Review-Alert-Bottom.png)

### 7. Error Handling

![Gmail Error Handling](screenshots/03-Gmail-Error-Handling.png)

### 8. Input & Output Data

**Patient Input Data**

![Patient Input Data](screenshots/10-Patient-Input-Data.png)

**Patient Output Data**

![Patient Output Data](screenshots/11-Patient-Output-Data.png)

## Classification & Routing Logic

The AI classifies submissions into four categories:

| Category | Description | Workflow Handling |
|---|---|---|
| Emergency | Potentially serious warning signs | Gmail alert |
| Needs_Review | Unclear, incomplete, or contradictory information | Gmail alert |
| Routine | Non-emergency dental concerns | Google Sheets |
| Inquiry | General questions or service-related requests | Google Sheets |

### Alert Conditions

The IF node uses the following conditions with **OR** logic:

- Urgency Score is greater than or equal to 8.
- Category equals `Needs_Review`.

If either condition is true, the workflow sends a Gmail notification and then saves the patient record to Google Sheets.

Otherwise, the submission is saved directly to Google Sheets.

## Technologies Used

- **n8n:** Workflow automation and integration
- **Google Gemini:** AI-powered classification and summarization
- **JavaScript:** Data parsing, validation, and processing
- **Webhook:** Patient submission intake
- **Gmail API:** Automated clinic email notifications
- **Google Sheets:** Patient record storage

## Testing & Validation

The workflow was tested using four different scenarios to verify its classification and routing behavior.

| Test Scenario | Category | Score | Expected Result |
|---|---|---:|---|
| 1 | Emergency | 9 | Gmail alert + Google Sheets |
| 2 | Needs_Review | 5 | Gmail alert + Google Sheets |
| 3 | Routine | 5 | Google Sheets only |
| 4 | Inquiry | 2 | Google Sheets only |

All four scenarios were tested to verify the expected routing behavior.

## Error Handling

A separate n8n error workflow is included in the repository.

It is designed to send an email notification when an execution error occurs, helping make workflow failures visible for review.

## Repository Structure

```text
Clinic-Triage-Automation/
│
├── workflow/
│   └── Clinic-Triage-Automation-Sanitized.json
│
├── error-workflow/
│   └── Main-Automations-Error-Workflow-Sanitized.json
│
├── data/
│   ├── Clinic-Triage-Input-Data.csv
│   └── Clinic-Triage-Output-Data.csv
│
├── screenshots/
│   ├── 01-Workflow-Overview.png
│   ├── 02-Successful-Execution.png
│   ├── 03-Gmail-Error-Handling.png
│   ├── 04-Emergency-Alert-Top.png
│   ├── 05-Emergency-Alert-Bottom.png
│   ├── 06-Needs-Review-Alert-Top.png
│   ├── 07-Needs-Review-Alert-Bottom.png
│   ├── 08-AI-Classification-Node.png
│   ├── 09-Prepare-Patient-Record.png
│   ├── 10-Patient-Input-Data.png
│   └── 11-Patient-Output-Data.png
│
└── README.md
```

## Setup & Configuration

The workflow JSON files are sanitized portfolio templates. They are not ready to run immediately after import.

To use the automation:

1. Install or access an n8n instance.
2. Import the main workflow JSON file.
3. Configure your own Google Gemini credentials.
4. Connect your Gmail account and configure the clinic recipient email.
5. Connect your Google Sheets account and select your own spreadsheet and worksheet.
6. Configure the webhook endpoint and submit test data.
7. Import and configure the separate error workflow if required.

**Note:** The exported workflows do not contain working credentials or private account configurations. You must configure your own integrations before execution.

## Safety & Data Privacy

- The AI is instructed to perform administrative classification, not clinical diagnosis.
- Patient-submitted text is treated as untrusted input.
- AI-generated output is validated before further processing.
- Emergency and unclear submissions are escalated for human review according to the configured routing rules.
- Human clinic staff remain responsible for all clinical decisions.
- The repository uses fictional sample data. Do not upload real patient information, personal medical records, or private credentials.

## Project Purpose

This project demonstrates practical skills in:

- AI-powered workflow automation
- API and webhook integration
- Prompt design and structured AI output
- JSON parsing and data validation
- Conditional workflow routing
- Error notification workflows
- Google Workspace integration
- Business process automation

---

**Built with n8n | Google Gemini | JavaScript | Gmail | Google Sheets**
