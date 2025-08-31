# Complete Setup Guide for All 5 N8N Workflows

## Prerequisites

Before setting up any workflow, ensure you have:

1. **N8N Instance Running**
   - Self-hosted N8N or N8N Cloud account
   - Access to workflow editor
   - Proper permissions to create workflows

2. **Email SMTP Configuration**
   - SMTP server details (Gmail, Outlook, or custom SMTP)
   - Authentication credentials
   - Sender email address configured

## General Email Setup Instructions

For all workflows, you'll need to configure email credentials:

1. Go to **Credentials** in your N8N instance
2. Click **Add Credential** → **SMTP**
3. Enter your SMTP details:
   - **Host**: smtp.gmail.com (for Gmail) or your SMTP server
   - **Port**: 587 (TLS) or 465 (SSL)
   - **Username**: Your email address
   - **Password**: Your email password or app password
   - **Security**: STARTTLS or SSL/TLS

---

## Workflow 1: Website Monitor & Alert System

### Prerequisites
- Target website URL to monitor
- Email credentials configured

### Setup Steps

1. **Import the JSON**
   - Copy the Website Monitor JSON
   - In N8N, go to Workflows → Import from JSON
   - Paste the JSON content

2. **Configure Nodes**

   **Cron Trigger Node:**
   - Set interval to run every 5 minutes: `*/5 * * * *`
   - Activate the trigger

   **HTTP Request Node:**
   - Update the URL to your website: `https://your-website.com`
   - Set timeout to 10 seconds
   - Method: GET

   **Email Nodes:**
   - Configure both "Send UP Alert" and "Send DOWN Alert" nodes
   - Update recipient email addresses
   - Customize email content as needed

3. **Testing**
   - Test with a known working URL first
   - Test with an invalid URL to verify DOWN alerts
   - Check email delivery

4. **Activation**
   - Save and activate the workflow
   - Monitor the executions tab for results

### Customization Options
- Adjust monitoring frequency
- Add multiple websites by duplicating the HTTP request section
- Customize alert messages
- Add SMS notifications using additional nodes

---

## Workflow 2: Database Backup Reminder System

### Prerequisites
- FTP/SFTP server access with backup files
- Database backup naming convention
- Email credentials

### Setup Steps

1. **Import JSON and Configure FTP**

   **FTP Node Configuration:**
   - Protocol: FTP or SFTP
   - Hostname: Your backup server address
   - Username: FTP username
   - Password: FTP password
   - Path: Directory where backups are stored

2. **Customize Backup Analysis**

   **Code Node:**
   - Update file extensions for your backup format (.sql, .dump, .bak)
   - Modify the backup file naming pattern
   - Adjust the timeframe for "recent" backups (currently 24 hours)

3. **Configure Email Recipients**
   - Update admin and DBA email addresses
   - Customize alert thresholds
   - Modify email templates

### Advanced Configuration
- Connect to cloud storage (AWS S3, Google Drive) instead of FTP
- Add backup size validation
- Include database-specific backup validation

---

## Workflow 3: Form Submission Auto-Responder

### Prerequisites
- Website form that can send POST requests
- Email credentials for sending confirmations

### Setup Steps

1. **Import JSON and Get Webhook URL**

   **Webhook Trigger:**
   - After importing, note the webhook URL from the trigger node
   - Configure your website form to POST to this URL

2. **Customize Form Processing**

   **Code Node - Form Processing:**
   - Update field names to match your form fields
   - Modify validation rules
   - Customize sanitization logic

3. **Configure Email Templates**

   **Confirmation Email:**
   - Update sender email address
   - Customize email template
   - Add your company branding

   **Admin Notification:**
   - Set admin email addresses
   - Configure internal notification format

4. **Website Form Integration**
   ```html
   <!-- Example HTML form -->
   <form action="YOUR_WEBHOOK_URL" method="POST">
     <input type="text" name="name" required>
     <input type="email" name="email" required>
     <textarea name="message" required></textarea>
     <button type="submit">Submit</button>
   </form>
   ```

### Testing
- Submit test forms with valid and invalid data
- Verify email delivery to both customer and admin
- Check webhook response handling

---

## Workflow 4: Social Media Mention Alert System

### Prerequisites
- Twitter API credentials (Bearer Token)
- Keywords/hashtags to monitor
- Email credentials

### Setup Steps

1. **Twitter API Setup**
   - Apply for Twitter API access at developer.twitter.com
   - Create a Twitter app and get Bearer Token
   - Configure credentials in N8N

2. **Configure Twitter Node**
   - Add Twitter credentials
   - Update search parameters:
     - Brand name keywords
     - Hashtags to monitor
     - Exclude retweets option
     - Result limit (default: 50)

3. **Customize Mention Processing**

   **Code Node:**
   - Update sentiment analysis keywords
   - Modify engagement thresholds
   - Customize mention categorization logic

4. **Configure Alert Recipients**
   - Set marketing team email addresses
   - Customize alert frequency
   - Modify email templates

### Alternative Social Platforms
- Replace Twitter node with Facebook, Instagram, or LinkedIn nodes
- Adjust the processing logic for different data structures
- Configure multiple social platform monitoring

---

## Workflow 5: Invoice Due Date Reminder System

### Prerequisites
- Invoice data source (Google Sheets, database, or CRM)
- Client email addresses in the data
- Email credentials

### Setup Steps

1. **Data Source Configuration**

   **Google Sheets Setup:**
   - Create a Google Sheets document with invoice data
   - Required columns:
     - Invoice Number
     - Client Name
     - Client Email
     - Amount
     - Due Date
     - Status
   - Get the document ID from the URL
   - Configure Google Sheets credentials in N8N

2. **Alternative Data Sources**
   - Replace Google Sheets node with MySQL, PostgreSQL, or Airtable
   - Update the Code node to match your data structure
   - Ensure date formatting matches your system

3. **Configure Reminder Logic**

   **Code Node:**
   - Update date parsing logic for your date format
   - Modify reminder categories (overdue, urgent, upcoming)
   - Adjust reminder timeframes
   - Customize currency formatting

4. **Email Configuration**
   - Update sender email address
   - Customize reminder email templates
   - Set appropriate finance team recipients
   - Configure payment portal links

### Data Structure Example
| Invoice Number | Client Name | Client Email | Amount | Due Date | Status |
|---------------|-------------|--------------|--------|----------|--------|
| INV-001 | ABC Corp | billing@abccorp.com | 1500.00 | 2024-02-15 | pending |
| INV-002 | XYZ Ltd | finance@xyzltd.com | 2750.00 | 2024-02-20 | paid |

---

## Common Troubleshooting

### Email Issues
- **Emails not sending**: Check SMTP credentials and firewall settings
- **Emails in spam**: Configure SPF, DKIM records for your domain
- **Authentication errors**: Use app passwords for Gmail/Outlook

### Webhook Issues
- **404 errors**: Verify webhook URL and HTTP method
- **CORS errors**: Configure proper headers in webhook response
- **Timeout issues**: Optimize Code node performance

### API Rate Limits
- **Twitter API**: Monitor rate limits, implement backoff strategies
- **Google Sheets**: Use batch operations, avoid frequent requests
- **FTP connections**: Implement connection pooling

### Data Processing
- **Date parsing errors**: Ensure consistent date formats
- **Memory issues**: Process large datasets in batches
- **Validation failures**: Implement comprehensive error handling

## Security Best Practices

1. **Credential Management**
   - Use N8N credential system, never hardcode passwords
   - Regularly rotate API keys and passwords
   - Implement least-privilege access

2. **Data Protection**
   - Sanitize all user inputs
   - Implement data retention policies
   - Use HTTPS for all connections

3. **Monitoring**
   - Set up execution logging
   - Monitor for failed executions
   - Implement alerting for critical failures

## Performance Optimization

1. **Workflow Efficiency**
   - Use appropriate trigger intervals
   - Implement conditional logic to reduce unnecessary executions
   - Optimize Code nodes for performance

2. **Resource Management**
   - Monitor workflow execution times
   - Implement batch processing for large datasets
   - Use workflow queuing for high-volume scenarios

3. **Scaling Considerations**
   - Consider N8N clustering for high availability
   - Implement database optimization for large datasets
   - Use external services for heavy processing tasks