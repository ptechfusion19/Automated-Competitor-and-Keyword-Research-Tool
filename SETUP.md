# Setup & Usage Guide

## Prerequisites

- **n8n instance** (cloud or self-hosted)
- **DataForSEO API account** with active subscription
- **OpenAI API account** with access to GPT-4.1-mini
- **Google Drive account** for report storage
- **SMTP email server** for sending reports

## Setup Instructions

### 1. Import Workflow to n8n

1. Open your n8n instance
2. Click **"Workflows"** → **"Import from File"**
3. Select `Automated Competitor & Keyword Research Tool.json`
4. The workflow will be imported with all nodes configured

### 2. Configure API Credentials

#### DataForSEO API
1. In n8n, go to **Credentials** → **Add Credential**
2. Select **HTTP Basic Auth**
3. Name: `DataForSeo`
4. Enter your DataForSEO credentials:
   - **User**: Your DataForSEO login
   - **Password**: Your DataForSEO password
5. Save the credential
6. Update the workflow nodes "Get Ranked Keywords", "Get Competitors", and "Get Keywords for Each Competitor" to use this credential

#### OpenAI API
1. In n8n, go to **Credentials** → **Add Credential**
2. Select **OpenAI API**
3. Enter your OpenAI API key
4. Save the credential
5. Update the "gpt" node in the workflow to use this credential

#### Google Drive
1. In n8n, go to **Credentials** → **Add Credential**
2. Select **Google Drive OAuth2 API**
3. Follow the OAuth2 setup process to connect your Google account
4. Grant permissions for Google Drive access
5. Save the credential
6. Update the "Google Drive" node to use this credential

#### SMTP Email
1. In n8n, go to **Credentials** → **Add Credential**
2. Select **SMTP**
3. Enter your SMTP server details:
   - **Host**: Your SMTP server (e.g., `smtp.gmail.com`)
   - **Port**: SMTP port (e.g., `587`)
   - **User**: Your email address
   - **Password**: Your email password or app password
   - **Secure**: Enable TLS/SSL as required
4. Save the credential
5. Update the "Send Report" and "Send Error" nodes to use this credential

### 3. Configure Google Drive Folder

1. Create a folder in Google Drive where reports will be stored
2. Copy the folder ID from the Google Drive URL
3. In the workflow, open the **"Google Drive"** node
4. Set the **Folder ID** to your created folder ID
5. Set **Drive** to "My Drive" or your preferred drive

### 4. Update Email Settings

1. Open the **"Send Report"** node
2. Update the `fromEmail` field to your sender email address
3. Open the **"Send Error"** node
4. Update the `fromEmail` field to your sender email address

### 5. Activate Workflow

1. In n8n, toggle the workflow to **Active**
2. The webhook will be available at: `https://your-n8n-instance.com/webhook/Automated-Competitor-Keyword-Research-Tool`

## Usage

### Method 1: Web Interface

1. Open `index.html` in a web browser or host it on a web server
2. Fill in the form:
   - **Website URL**: Enter the target website to analyze (e.g., `https://example.com`)
   - **Email Address**: Enter the email where the report should be sent
3. Click **"Start Research"**
4. Wait for the confirmation message
5. The report will be generated and sent to your email within a few minutes

**Note**: Update the webhook URL in `index.html` (line 400) if using a different n8n instance.

### Method 2: Direct API Call

Send a POST request to the webhook endpoint:

**Endpoint**: `https://your-n8n-instance.com/webhook/Automated-Competitor-Keyword-Research-Tool`

**Request Headers**:
```
Content-Type: application/json
```

**Request Body**:
```json
{
  "target_url": "https://example.com",
  "email": "user@example.com",
  "location_name": "United Kingdom",
  "language_name": "English",
  "location_code": 2826,
  "language_code": "en"
}
```

**Example using cURL**:
```bash
curl -X POST https://your-n8n-instance.com/webhook/Automated-Competitor-Keyword-Research-Tool \
  -H "Content-Type: application/json" \
  -d '{
    "target_url": "https://example.com",
    "email": "user@example.com",
    "location_name": "United Kingdom",
    "language_name": "English",
    "location_code": 2826,
    "language_code": "en"
  }'
```

**Example using JavaScript**:
```javascript
fetch('https://your-n8n-instance.com/webhook/Automated-Competitor-Keyword-Research-Tool', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    target_url: 'https://example.com',
    email: 'user@example.com',
    location_name: 'United Kingdom',
    language_name: 'English',
    location_code: 2826,
    language_code: 'en'
  })
});
```

## Configuration Options

### Supported Locations

The workflow supports the following locations (with their codes):

- United States: `2840`
- United Kingdom: `2826`
- Canada: `2124`
- Australia: `2036`
- Germany: `2276`
- France: `2250`
- Spain: `2724`
- Italy: `2380`
- Netherlands: `2528`
- India: `2356`

### Supported Languages

- English: `en`
- Spanish: `es`
- French: `fr`
- German: `de`
- Italian: `it`
- Portuguese: `pt`
- Dutch: `nl`
- Russian: `ru`
- Chinese: `zh`
- Japanese: `ja`

### Filtering Parameters

The workflow uses the following default filtering criteria (can be modified in the "Filter High Volume Keywords" node):

- **Minimum Search Volume**: 100 monthly searches
- **Maximum Rank**: Top 20 positions

To change these values, edit the JavaScript code in the "Filter High Volume Keywords" node:

```javascript
const MIN_SEARCH_VOLUME = 100; // Change this value
const MAX_RANK = 20; // Change this value
```

### Keyword Limits

- **Target Site Keywords**: 100 keywords fetched
- **Competitors Identified**: Top 10 competitors
- **Keywords per Competitor**: Up to 100 keywords

## Troubleshooting

### Workflow Not Triggering

- Verify the workflow is **Active** in n8n
- Check the webhook URL is correct
- Ensure the webhook path matches: `Automated-Competitor-Keyword-Research-Tool`

### API Errors

- **DataForSEO Errors**: Check your API quota and account status
- **OpenAI Errors**: Verify your API key is valid and has sufficient credits
- Check the error email notification for detailed error messages

### Email Not Received

- Verify SMTP credentials are correct
- Check spam/junk folder
- Ensure the email address in the request is valid
- Check n8n execution logs for email sending errors

### Google Drive Upload Fails

- Verify Google Drive OAuth2 credentials are valid
- Check folder ID is correct and accessible
- Ensure the Google account has sufficient storage space

### No Keywords Found

- The target website may have limited keyword data in DataForSEO
- Try a different location/language combination
- Check if the website is indexed and has organic search presence

## Report Delivery

Once the workflow completes successfully:

1. A markdown report is generated and uploaded to Google Drive
2. An email notification is sent to the provided email address
3. The email contains a direct link to view the report in Google Drive
4. Report filename format: `SEO Keyword Gap Report_<website>_<date>.md`

## Support

For issues or questions:
- Check n8n execution logs for detailed error messages
- Review DataForSEO API documentation for API-specific issues
- Verify all credentials are properly configured and active

