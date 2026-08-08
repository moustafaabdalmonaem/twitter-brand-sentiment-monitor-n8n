# Twitter Brand Sentiment Monitor (AI Sentiment Analysis)

An automated brand-monitoring workflow built with [n8n](https://n8n.io). It tracks recent mentions of your brand on **Twitter/X**, uses Claude AI to analyze whether each mention is positive, neutral, or negative, logs everything to a Google Sheet, and emails you a weekly summary report.

## What it does

This workflow has **two parts** that run on their own schedules:

### Part 1 — Continuous monitoring (every 6 hours)

1. **Searches recent tweets** mentioning your brand name using the Twitter API v2 recent search endpoint.
2. **Splits the results** into individual mentions.
3. **Sends each mention to Claude AI**, which classifies it as `positive`, `neutral`, or `negative` with a short reason.
4. **Logs every mention** into a Google Sheet.

```
Every 6 Hours (Schedule Trigger)
        │
        ▼
Search Brand Mentions (Twitter)
        │
        ▼
Split Into Individual Mentions
        │
        ▼
Analyze Sentiment (Claude AI)
        │
        ▼
Parse Sentiment Result
        │
        ▼
Log Mention to Sheet (Google Sheets)
```

### Part 2 — Weekly report (every Monday at 9 AM)

1. **Reads all logged mentions** from the Google Sheet.
2. **Calculates statistics**: total mentions, and the count/percentage of positive, neutral, and negative posts.
3. **Emails a summary report**.

```
Every Monday at 9 AM (Schedule Trigger)
        │
        ▼
Get This Week's Mentions (Google Sheets)
        │
        ▼
Summarize Weekly Stats
        │
        ▼
Send Weekly Report Email (Gmail)
```

## Requirements

- An [n8n](https://n8n.io) instance (cloud or self-hosted).
- A **Twitter/X Developer account** with API access (at least the Basic tier) and a Bearer Token for the recent search endpoint.
- An **Anthropic (Claude) API key** — from [console.anthropic.com](https://console.anthropic.com).
- A Google account with **Google Sheets API** access configured in n8n.
- A **Gmail** account connected in n8n for the weekly report email.
- A Google Sheet with a tab named `Mentions` and the following columns:
  - `Date`, `PostText`, `Sentiment`, `Reason`

## Setup instructions

1. Import `twitter-brand-sentiment-monitor-workflow.json` into your n8n instance:
   `Workflows → Import from File`.
2. Open **Search Brand Mentions (Twitter)** and:
   - Replace `YourBrandName` in the `query` parameter with your actual brand name or search terms.
   - Add your Twitter Bearer Token credential.
3. Add your **Claude API key** as an HTTP Header Auth credential (header name: `x-api-key`) and select it in **Analyze Sentiment (Claude AI)**.
4. Open **Log Mention to Sheet** and **Get This Week's Mentions** and:
   - Set your Google Sheets credentials.
   - Replace `PUT_YOUR_GOOGLE_SHEET_ID_HERE` with your actual Google Sheet ID (in both nodes).
5. Open **Send Weekly Report Email** and set your Gmail credentials and recipient address.
6. Activate the workflow. Both schedules will now run automatically.

## Customization ideas

- Track multiple brand names or keywords by adjusting the search query (e.g. brand name + common misspellings).
- Send an instant alert (Slack/Telegram) whenever a strongly negative mention is detected, instead of waiting for the weekly report.
- Add a chart of sentiment trends over time using the logged historical data.
- Store direct links to each tweet (`id` field) so you can jump straight to negative mentions and respond quickly.
- Filter out low-engagement or spam accounts before running sentiment analysis to save on API costs.
- Extend to also track replies to your own brand account, not just external mentions.

## License

Free to use and modify for personal or commercial projects.

<img width="1280" height="629" alt="Workflow 7" src="https://github.com/user-attachments/assets/afecf0c4-f6e5-45ce-9ab1-0b8e09f4830e" />
