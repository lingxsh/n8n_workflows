# 5 N8N Workflows with Email Output - Complete Guide

## Workflow 1: Website Monitor & Alert System

### Purpose
Monitors a website's availability and sends email alerts when the site goes down or comes back online.

### Setup Requirements
- SMTP email credentials
- Target website URL to monitor
- Cron trigger for scheduled monitoring

### Workflow Explanation
1. **Cron Trigger**: Runs every 5 minutes to check website status
2. **HTTP Request**: Makes a GET request to the target website
3. **IF Node**: Checks if the response status is 200 (OK)
4. **Email Node**: Sends alerts based on website status

### Configuration Steps
1. Set up Cron trigger for `*/5 * * * *` (every 5 minutes)
2. Configure HTTP Request node with your target URL
3. Set up IF node to check `{{$json.statusCode}} === 200`
4. Configure email credentials in Email node
5. Set different email templates for up/down alerts

---

## Workflow 2: Database Backup Reminder System

### Purpose
Monitors database backup files and sends email reminders if backups are missing or outdated.

### Setup Requirements
- FTP/SFTP credentials or file system access
- Email SMTP settings
- Database backup directory path

### Workflow Explanation
1. **Schedule Trigger**: Runs daily to check backup status
2. **FTP Node**: Connects to backup server and lists files
3. **Code Node**: Analyzes backup files and determines if recent backups exist
4. **IF Node**: Checks if backup is recent (within last 24 hours)
5. **Email Node**: Sends reminder if backup is missing or old

### Configuration Steps
1. Set up Schedule trigger for daily execution
2. Configure FTP node with server credentials
3. Set up Code node to parse file dates and check recency
4. Configure IF condition for backup age validation
5. Set up email templates for different alert types

---

## Workflow 3: Form Submission Auto-Responder

### Purpose
Processes form submissions from a webhook and sends personalized confirmation emails to users.

### Setup Requirements
- Webhook URL endpoint
- Email SMTP configuration
- Form field mapping

### Workflow Explanation
1. **Webhook Trigger**: Receives form submission data
2. **Code Node**: Processes and validates form data
3. **IF Node**: Checks if email address is valid
4. **Email Node**: Sends personalized confirmation email
5. **HTTP Response**: Returns success message to form

### Configuration Steps
1. Set up Webhook trigger and note the webhook URL
2. Configure Code node to validate and format form data
3. Set up email validation in IF node
4. Create personalized email template with form data
5. Configure HTTP Response for form feedback

---

## Workflow 4: Social Media Mention Alert System

### Purpose
Monitors social media mentions of your brand/keywords and sends email summaries.

### Setup Requirements
- Twitter API credentials (or other social media APIs)
- Email SMTP settings
- Keywords/hashtags to monitor

### Workflow Explanation
1. **Interval Trigger**: Runs every hour to check for mentions
2. **Twitter Node**: Searches for tweets containing specified keywords
3. **Code Node**: Filters and formats mention data
4. **IF Node**: Checks if there are new mentions
5. **Email Node**: Sends summary email with mention details

### Configuration Steps
1. Set up Interval trigger for hourly execution
2. Configure Twitter API credentials
3. Set up search parameters for keywords/hashtags
4. Create mention filtering logic in Code node
5. Design email template with mention summaries

---

## Workflow 5: Invoice Due Date Reminder System

### Purpose
Monitors invoice due dates and sends automated reminder emails to clients.

### Setup Requirements
- Database or spreadsheet with invoice data
- Email SMTP configuration
- Client contact information

### Workflow Explanation
1. **Schedule Trigger**: Runs daily to check due dates
2. **Spreadsheet/Database Node**: Retrieves invoice data
3. **Code Node**: Calculates days until due date
4. **IF Node**: Filters invoices due within specified timeframe
5. **Email Node**: Sends reminder emails to clients

### Configuration Steps
1. Set up daily Schedule trigger
2. Configure database/spreadsheet connection
3. Create date calculation logic in Code node
4. Set up filtering for due dates (e.g., 7 days, 3 days, overdue)
5. Create professional reminder email templates