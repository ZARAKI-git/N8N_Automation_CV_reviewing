# Instagram Recruiting Funnel — CV Review Automation (n8n)

> An n8n workflow that turns an Instagram reel into a fully automated recruiting funnel — DMs applicants, parses their CVs with Claude, triages and ranks them, and logs everything to Google Sheets.

<p>
  <img alt="n8n" src="https://img.shields.io/badge/n8n-Workflow-EA4B71?logo=n8n&logoColor=white">
  <img alt="Anthropic" src="https://img.shields.io/badge/Anthropic-Claude-D97757?logo=anthropic&logoColor=white">
  <img alt="Instagram" src="https://img.shields.io/badge/Instagram-Graph%20API-E4405F?logo=instagram&logoColor=white">
  <img alt="Google Sheets" src="https://img.shields.io/badge/Google%20Sheets-Applicants-34A853?logo=googlesheets&logoColor=white">
</p>

## Overview

This repository contains an [n8n](https://n8n.io) automation that runs a two-stage
Instagram recruiting funnel. It listens for comments on a specific reel, opens a DM
conversation with interested candidates, then uses Claude to read their CV and portfolio,
score and rank them, and record the results in a Google Sheet — all hands-free.

The workflow is built defensively: external calls use `continueOnFail` / try-catch so a
single failure never breaks the funnel.

## How It Works

### Flow 1 — Comment trigger (locked to one reel)
Listens for Instagram comment webhooks. When someone comments the keyword **"graphics"**
(case-insensitive) on the target reel, it sends an opening DM via the Instagram Private
Reply API (using the `comment_id` as the recipient, per Meta's first-message spec).

### Flow 2 — Conversation, extraction, AI triage & ranking
Triggered by Instagram DM webhooks. It:
1. Parses the applicant's reply with **Claude**.
2. Fetches the CV and portfolio links.
3. Runs an **AI triage** to evaluate the candidate.
4. Appends the result to a **Google Sheet**.
5. Recomputes the ranking across all applicants.
6. Confirms receipt back to the applicant.

## Nodes Used

`Webhook` · `Respond to Webhook` · `IF` · `Code` · `Anthropic (Claude)` ·
`HTTP Request` · `Split Out` · `Google Sheets`

## Getting Started

### Prerequisites
- An [n8n](https://n8n.io) instance (self-hosted or cloud)
- A Meta/Instagram app with the Instagram Graph API + webhooks configured
- An Anthropic API key
- A Google account with Sheets access

### Import the workflow

1. In n8n, go to **Workflows → Import from File**.
2. Select [`IG_Recruiting_Funnel_COMPLETE.json`](./IG_Recruiting_Funnel_COMPLETE.json).
3. Configure credentials for **Anthropic**, **Google Sheets**, and the **Instagram Graph API**.
4. Replace the placeholders (see below).
5. Subscribe your Instagram app webhooks to the workflow's webhook URLs.
6. Activate the workflow.

## Configuration Notes

- Set `PLACEHOLDER_REEL_MEDIA_ID` to your target reel's media ID.
- Adjust the trigger keyword (default: **"graphics"**) if needed.
- Provide your Google Sheet ID and Instagram/Meta access tokens.
- The in-canvas sticky notes document each flow and its placeholders.

## License

Proprietary — all rights reserved.
