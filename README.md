# CivicPal

A smart civic complaint management system that automatically processes, classifies, and organizes citizen complaints through email, NLP analysis, and a REST API.

## Overview

CivicPal automates the workflow of handling civic complaints by:
- Fetching complaints from email inboxes
- Using NLP to classify complaints into relevant categories
- Mapping complaints to constituencies and departments
- Storing complaints in a structured format
- Providing an API to query and track complaint status
- Monitoring and alerting on critical issues

## Features

✨ **Key Features:**
- **Email Integration**: Automatically fetches complaints from Gmail
- **NLP Classification**: Intelligent categorization using semantic similarity
- **Constituency Mapping**: Automatically identifies the correct constituency for each complaint
- **Data Persistence**: Stores all complaints in JSON format
- **REST API**: Query complaints and statistics by constituency
- **Status Tracking**: Monitor resolution status of complaints
- **Alerts & Monitoring**: Get notified of critical issues
- **Email Replies**: Send automated responses to complainants

## Project Structure

```
civicpal/
├── api/                          # Flask REST API
│   └── app.py                   # API endpoints for complaints and stats
├── data_storage/                # Data management
│   ├── complaints.json          # Complaint database
│   └── store_complaint.py       # Complaint storage logic
├── email_reader/                # Email integration
│   ├── fetch_emails.py          # Gmail IMAP client
│   └── reply_emails.py          # Email response handler
├── nlp_classifier/              # NLP processing
│   └── process_email.py         # Complaint classification engine
├── alerts/                       # Alerting system
│   └── monitor_alerts.py        # Alert monitoring logic
├── test_agent.py               # Testing script
└── README.md                    # This file
```

## Supported Complaint Categories

- 💧 **Water**: No water supply, low pressure, dry taps
- ⚡ **Electricity**: Power cuts, no power, electrical issues
- 🗑️ **Garbage**: Trash collection, overflowing bins, waste management
- 🛣️ **Road**: Potholes, broken roads, street damage
- 🚰 **Sanitation**: Sewage, drainage issues, unhygienic areas
- 🐕 **Stray Dogs**: Stray dog menace, animal concerns

## Technology Stack

- **Backend**: Python 3.x
- **Web Framework**: Flask
- **NLP**: Sentence Transformers (all-MiniLM-L6-v2)
- **Email**: IMAP (Gmail)
- **Data Storage**: JSON
- **Dependencies**: See requirements.txt

## Setup Instructions

### Prerequisites
- Python 3.7+
- Gmail account with App Password enabled
- pip (Python package manager)

### Installation

1. **Clone or download the project**
   ```bash
   cd civicpal
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Configure Gmail access**
   - Enable 2-factor authentication on your Gmail account
   - Generate an [App Password](https://myaccount.google.com/apppasswords)
   - Update credentials in `email_reader/fetch_emails.py`:
     ```python
     EMAIL = "your-email@gmail.com"
     PASSWORD = "your-app-password"
     ```

4. **Create the data storage directory**
   ```bash
   mkdir -p data_storage
   ```

## Usage

### Running the Test Agent

Test the complaint processing pipeline:

```bash
python test_agent.py
```

This will:
1. Process a sample complaint
2. Classify it using NLP
3. Store it in the database
4. Display the analysis results

### Starting the API Server

Launch the Flask REST API:

```bash
python api/app.py
```

The server runs on `http://localhost:5000`

### API Endpoints

#### Get Complaints by Constituency
```
GET /api/complaints?constituency=<constituency_name>
```

**Response:**
```json
[
  {
    "timestamp": "2026-05-20T10:30:00",
    "subject": "Need urgent help",
    "body": "No water supply in our area",
    "category": "Water",
    "constituency": "Dasarahalli",
    "department": "Water Supply",
    "confidence": 0.95,
    "status": "unresolved"
  }
]
```

#### Get Statistics by Constituency
```
GET /api/stats?constituency=<constituency_name>
```

**Response:**
```json
{
  "total": 15,
  "resolved": 10,
  "unresolved": 5
}
```

### Fetching Complaints from Email

Run the email reader to fetch new complaints:

```bash
python email_reader/fetch_emails.py
```

This will connect to Gmail, fetch new emails, classify them, and store them in the database.

## Complaint Data Structure

Each complaint is stored with the following fields:

```json
{
  "timestamp": "2026-05-20T10:30:00.123456",
  "subject": "Water supply issue",
  "body": "No water since yesterday evening",
  "category": "Water",
  "constituency": "Dasarahalli",
  "department": "Water Supply",
  "confidence": 0.95,
  "status": "unresolved"
}
```

## Configuration

### Email Configuration
Edit `email_reader/fetch_emails.py` to set:
- Email address
- App password
- IMAP server settings

### NLP Model
The system uses `all-MiniLM-L6-v2` from Hugging Face. To use a different model, edit `nlp_classifier/process_email.py`

### Category Mappings
Customize complaint categories in `nlp_classifier/process_email.py`:
```python
category_examples = {
    "Water": [...],
    "Electricity": [...],
    # Add more categories
}
```

## Workflow

```
Email → Fetch → Classify → Store → API Query
```

1. **Email Fetch**: Gmail IMAP client retrieves new emails
2. **Classify**: NLP engine analyzes subject/body and assigns category, constituency, department
3. **Store**: Complaint is saved to `complaints.json` with metadata
4. **API**: REST endpoints allow querying and tracking complaints

## Future Enhancements

- [ ] Database integration (PostgreSQL, MongoDB)
- [ ] Web dashboard for complaint tracking
- [ ] Automated department assignments
- [ ] Multi-language support
- [ ] Integration with municipal systems
- [ ] Machine learning model improvements
- [ ] Complaint escalation workflows
- [ ] SMS notifications

## Troubleshooting

### Gmail Connection Issues
- Ensure 2FA is enabled on Gmail
- Generate a new App Password
- Check IMAP is enabled in Gmail settings

### NLP Model Download
First run will download the Sentence Transformer model (~100MB). Ensure internet connection is available.

### JSON Encoding Errors
Ensure complaint data is properly encoded as UTF-8.

## Contributing

Feel free to submit issues and enhancement requests!

## License

This project is open source and available for civic engagement.

## Support

For issues or questions, please refer to the inline code documentation or create an issue in the repository.

---

**Last Updated**: May 2026

Made with ❤️ for civic engagement and community improvement.
