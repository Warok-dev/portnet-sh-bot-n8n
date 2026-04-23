# PortNet SH Code Lookup Bot — n8n Workflow

An intelligent Telegram chatbot built with n8n, OpenAI GPT-4, and Pinecone that allows users to query customs HS (Harmonized System) codes in real time. Developed and deployed during an internship at PortNet S.A., Morocco's national port logistics platform.

---

## What it does

Users send a message to a Telegram bot — either a product name (e.g. "avocats du Mexique") or a direct HS code — and the bot automatically:

1. Authenticates the user (access control via Telegram identity)
2. Classifies the input (numeric code vs. product name vs. invalid)
3. Routes to the appropriate pipeline:
   - Direct HS code → sub-workflow for detailed lookup
   - Product name → AI Agent extracts product, queries Pinecone vector store, returns matching HS codes
   - Invalid input → prompts user to retry
4. Responds in Telegram with the result

---

## Tech Stack

| Component | Technology |
|---|---|
| Workflow Automation | n8n |
| Messaging Interface | Telegram Bot API |
| AI Agent | OpenAI GPT-4 (via n8n LangChain nodes) |
| Vector Database | Pinecone |
| Embeddings | OpenAI Embeddings |
| Memory | n8n Buffer Window Memory |
| Language | Python (custom classification node) |

---

## Architecture

```
Telegram Message
      |
      v
 Access Control (If node)
      |
      v
 Input Classification (Python Code node)
      |
   +--+------------------+
   |                     |
   v                     v
Numeric Code         Product Name
   |                     |
   v                     v
Sub-workflow         AI Agent (GPT-4)
(HS lookup)              |
                         v
                  Pinecone Vector Store
                  (HS code database)
                         |
                         v
                  Telegram Response
```

---

## How to use

### Prerequisites
- n8n instance (self-hosted or cloud)
- Telegram Bot Token
- OpenAI API Key
- Pinecone API Key + index with HS codes data

### Setup
1. Import `portnet_sh_bot_workflow.json` into your n8n instance
2. Configure credentials in n8n:
   - Telegram API → add your bot token
   - OpenAI API → add your API key
   - Pinecone API → add your API key + index name
3. In the If node (access control), replace `YOUR_LAST_NAME` with your filter value
4. Link the sub-workflow IDs to your own sub-workflows
5. Activate the workflow

---

## Files

```
portnet_sh_bot_workflow.json   — Main n8n workflow (credentials removed)
README.md
```

---

## Author

Mohammed Ahouzi
Computer Engineering Student — University of Ottawa
LinkedIn: linkedin.com/in/mohammed-ahouzi
GitHub: github.com/Warok-dev

---

## Note

Credentials have been removed from the workflow JSON. You must configure your own API keys via n8n's credential manager before running this workflow.
