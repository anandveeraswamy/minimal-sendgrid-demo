# Minimal SendGrid Email Sender

A bare-bones standalone Python demo to send a single email using SendGrid.

## Prerequisites

- Python 3.8+
- Windows PowerShell (or bash on macOS/Linux)

## SendGrid Account Setup

### Step 1: Create SendGrid Account

1. Go to [SendGrid.com](https://sendgrid.com/) and sign up for a free account
2. Free tier includes **100 emails/day** which is sufficient for development and small projects
3. Complete email verification process

### Step 2: Verify Sender Identity

**Important:** SendGrid requires sender verification before sending emails.

#### Single Sender Verification (Recommended for Development)

1. In SendGrid dashboard, go to **Settings** → **Sender Authentication**
2. Click **Verify a Single Sender**
3. Fill in the form:
   - **From Name:** Your app name (e.g., "Todo App")
   - **From Email Address:** Your verified email (e.g., yourname@gmail.com)
   - **Reply To:** Same or different email
   - **Company/Organization:** Your org name
   - **Address, City, State, ZIP, Country:** Required fields
4. Click **Create** and check your email for verification link
5. Click the verification link in the email

### Step 3: Create API Key

1. In SendGrid dashboard, go to **Settings** → **API Keys**
2. Click **Create API Key**
3. Name: `Django Todo App` (or any descriptive name)
4. Permissions: Select **Restricted Access**
   - Enable **Mail Send** → **Full Access**
   - All other permissions can remain off
5. Click **Create & View**
6. **IMPORTANT:** Copy the API key immediately and store it securely
   - You won't be able to see it again
   - Format: `SG.xxxxxxxxxxxxxxxx.xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

---

## Setup Steps

### Step 4: Create Project Folder & Virtual Environment

```powershell
python -m venv venv
source venv/scripts/activate
```

### Step 5: Install SendGrid Package

```powershell
pip install sendgrid==6.12.5 python-http-client==3.3.7 python-dotenv
```

### Step 6: Set Environment Variables

```powershell
Create a .env file with the following environment variable
SENDGRID_API_KEY="SG.your_key_here"
FROM_EMAIL="your-verified-email@example.com"
TO_EMAIL="recipient@example.com"
```

### Step 7: Create `send_email.py`

```python
import os
from sendgrid import SendGridAPIClient
from sendgrid.helpers.mail import Mail
from dotenv import load_dotenv
load_dotenv()


api_key = os.getenv("SENDGRID_API_KEY")
from_email = os.getenv("FROM_EMAIL")
to_email = os.getenv("TO_EMAIL")

message = Mail(
    from_email=from_email,
    to_emails=to_email,
    subject="Hello from SendGrid",
    html_content="<p>This is a simple SendGrid test email.</p>",
)

try:
    sg = SendGridAPIClient(api_key)
    response = sg.send(message)
    print(f"Sent. Status: {response.status_code}")
except Exception as exc:
    print(f"Send failed: {exc}")
```

### Step 8: Run It

```powershell
python send_email.py
```

Output should be:
```
Sent. Status: 202
```

Status `202` = email queued successfully.

## Customization

Change the `Mail()` parameters:
- `subject`: Email title
- `html_content`: HTML body (or use `plain_text_content` for plain text)
- `to_emails`: Single email or list of emails

## That's It!

This is the absolute simplest SendGrid sender. No frameworks, no DB, no auth—just pure email dispatch.
