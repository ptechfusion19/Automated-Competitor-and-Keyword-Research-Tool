                                               ## Automated Competitor & Keyword Research Tool

An automated SEO tool that analyzes a website's keyword gaps by comparing it against top organic competitors and generates a comprehensive markdown report delivered via email.

### Features

- **Automatic competitor identification** - Discovers top 10 organic search competitors
- **Keyword gap analysis** - Identifies high-opportunity keywords competitors rank for but the target site doesn't
- **AI-powered reporting** - Generates structured markdown reports using GPT-4.1-mini
- **Automated delivery** - Uploads report to Google Drive and sends email notification

### Input Requirements

The tool accepts the following inputs via web interface or API:

- **Website URL** (required) - Target website to analyze
- **Email Address** (required) - Where to send the report
- **Location** (default: United Kingdom) - Geographic location for keyword analysis
- **Language** (default: English) - Language for keyword analysis

### Workflow

1. **Target Site Analysis**
   - Fetches up to 100 keywords for the target website
   - Extracts top 50 keywords by search volume for competitor discovery

2. **Competitor Identification**
   - Uses target site's top keywords to find organic competitors via DataForSEO API
   - Selects top 10 competitors based on SERP overlap

3. **Competitor Keyword Collection**
   - Fetches up to 100 keywords per competitor
   - Aggregates and deduplicates competitor keywords

4. **Gap Analysis**
   - Compares competitor keywords against target site keywords
   - Identifies keywords where competitors rank but target site doesn't

5. **Filtering**
   - Filters for high-opportunity keywords:
     - Minimum search volume: **100 monthly searches**
     - Maximum rank: **Top 20 positions**

6. **Report Generation**
   - Generates markdown report using GPT-4.1-mini
   - Includes: keyword metrics, competitor analysis, traffic potential, and SEO recommendations

7. **Delivery**
   - Uploads markdown file to Google Drive
   - Sends email notification with report link

### Report Contents

The generated report includes:

- **Key Statistics** - Total gap keywords, potential search volume, average difficulty, average CPC
- **Top Gap Keywords** - Grouped by competitor with:
  - Keyword text
  - Search volume
  - Keyword difficulty (KD)
  - Estimated traffic potential
- **Next Steps** - Prioritized SEO recommendations

### Web Interface

Access the tool via the web interface at `index.html` which provides a simple form to submit analysis requests.

### API Integration

**Production Webhook**: `https://n8n.programmx.com/webhook/Automated-Competitor-Keyword-Research-Tool`

**Request Format** (POST):
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

### Technology Stack

- **Automation Platform**: n8n
- **SEO Data API**: DataForSEO
- **AI Model**: GPT-4.1-mini (OpenAI)
- **Storage**: Google Drive
- **Email**: SMTP

### Error Handling

The workflow includes comprehensive error handling:
- Validates API responses at each step
- Sends error notifications via email if DataForSEO API calls fail
- Handles missing or invalid keyword data gracefully
