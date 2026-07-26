# email_adk_agent

An AI-powered email agent built with [Google's Agent Development Kit (ADK)](https://google.github.io/adk-docs/) that drafts and sends emails on your behalf. Give it a short natural-language instruction, and the agent uses an LLM (via [LiteLLM](https://docs.litellm.ai/)) to generate a subject and body, confirms the draft with you, and sends it through the **Gmail API** using OAuth 2.0.

## How It Works

1. **User Instruction** — You describe the email you want to send (e.g. *"send a test email to someone@example.com"*).
2. **Draft Generation** — The agent calls an LLM through LiteLLM to generate a subject and body.
3. **Confirmation** — The agent shows you the draft and asks for confirmation (or edits) before sending.
4. **Authentication** — OAuth 2.0 credentials are validated/refreshed against Google.
5. **Send** — The `send_email` tool calls the Gmail API's `messages.send` endpoint to deliver the email.

## Project Structure

```
email_adk_agent/
├── agent.py             # Agent definition, instructions, and the send_email tool
├── requirements.txt     # Python dependencies
├── .gitignore            # Excludes credentials, tokens, and env files from version control
└── .github/workflows/    # GitHub Actions workflow (Pages deployment)
```

## Requirements

- Python 3.10+
- A Google Cloud project with the **Gmail API** enabled
- An OAuth 2.0 Client ID (Desktop app) with the `gmail.send` scope

## Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/bishaltm567/email_adk_agent.git
   cd email_adk_agent
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Add your Google OAuth credentials**
   - Download your OAuth client credentials from [Google Cloud Console](https://console.cloud.google.com/) → APIs & Services → Credentials.
   - Save the file as `credentials.json` in the project root (this file is git-ignored and must **never** be committed).
   - On first run, the agent will open a browser window to authenticate and will generate a local `token.json` to persist your refresh token — also git-ignored.

4. **Configure environment variables**
   - Create a `.env` file in the project root with any required API keys (e.g. your LLM provider key for LiteLLM).

5. **Run the agent**
   ```bash
   adk web
   ```
   This launches the ADK Dev UI (typically at `http://127.0.0.1:8000`) where you can chat with the `email_agent` and watch it draft and send emails in real time.

   Alternatively, run it directly in the terminal:
   ```bash
   adk run email_adk_agent
   ```

## Security Notes

- `credentials.json`, `token.json`, and `.env` are excluded via `.gitignore` and should never be pushed to version control.
- The Gmail OAuth scope used (`gmail.send`) only allows sending mail — it cannot read your inbox.
- If credentials are ever exposed, revoke access at [Google Account Permissions](https://myaccount.google.com/permissions) and regenerate the client secret in Google Cloud Console.

## Tech Stack

- **google-adk** — agent orchestration framework
- **litellm** — unified interface for calling different LLM providers
- **google-api-python-client**, **google-auth-httplib2**, **google-auth-oauthlib** — Gmail API + OAuth 2.0
- **python-dotenv** — environment variable management

## Future Improvements

- Support for email threads and replies, not just new messages
- A review/edit step before sending (beyond simple yes/no confirmation)
- Attachment support and rich HTML formatting
- Multi-language draft generation

## Author

**Bishal TM**
Project submitted for the Summer 2026 Extra Course Skill Development Cohort — Phoenix College of Management and IT.
