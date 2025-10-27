# AI Fact-Checking Agent for Google Docs

This project integrates a powerful, custom-built AI agent from **Google Cloud Vertex AI** directly into **Google Docs**, enabling users to audit and verify document content with a single click.

It transforms Google Docs from a simple word processor into an intelligent editor that can actively combat misinformation by leveraging Google's Gemini models and Google Search.

![image url](https://github.com/balaji200528/Google-Docs-Agent-with-ADK/blob/09b1f63eb06afe5a14e5ff0fc8d30341fab39ec1/assets/image.png)
---

## 🎯 Core Problem & Solution

In an age of rapid information spread, misinformation can be easily embedded in documents. This tool provides a seamless solution by bringing AI-powered fact-checking directly into the user's writing workflow.

Instead of manually copying text to a search engine, the user can trigger a full document audit. The agent identifies all factual claims, verifies them against credible sources, and provides a concise report, all within the Google Doc.

---

## ✨ Key Features

The backend agent is a sophisticated multi-step AI system built with the Vertex AI Agent Development Kit (ADK). Its primary capabilities include:

* **Claim Extraction:** Intelligently scans the entire document to identify and pull out all statements that are factual claims.
* **Real-time Verification:** Uses the **Google Search API** to find reliable information and evidence related to each claim.
* **Verdict Classification:** Assigns a clear verdict to each claim:
    * ✅ **True:** Supported by credible sources.
    * ⚠️ **Misleading / Partially True:** Contains some correct information but is out of context or incomplete.
    * ❌ **False:** Contradicted by credible evidence.
    * ❓ **Unverifiable:** Insufficient information to confirm or deny.
* **Summarized Reporting:** Compiles all findings into a comprehensive, easy-to-read report, complete with reasoning for each verdict.

---

## 🏗️ Architecture & Technology Stack

This project connects Google Workspace to Google Cloud using a robust, event-driven architecture.

![image alt](https://github.com/balaji200528/Google-Docs-Agent-with-ADK/blob/e803a30ba4cb5936e4d01566438302361992f5ef/assets/Screenshot%202025-10-27%20161224.png)

### **How It Works**

1.  **Frontend (Google Docs):** A user writes a document and clicks a custom "AI Audit" menu button.
2.  **Middleware (Google Apps Script):**
    * The `onOpen()` trigger in `Code.gs` creates the custom menu.
    * The audit function captures the document's text and sends it to the Vertex AI endpoint using `UrlFetchApp`.
    * The `AIVertex.gs` file handles secure authentication using a Service Account and the `OAuth2` library.
3.  **Backend (Vertex AI Agent Engine):**
    * The deployed ADK agent (`agent.py`) receives the text as a request.
    * The agent orchestrates a series of tasks using Gemini to extract claims, the Google Search tool to verify them, and Gemini again to summarize the findings.
4.  **Return to Frontend:** The final formatted report is sent back to Apps Script, which then inserts the text directly into the user's active Google Doc.

### **Technology Stack**

* **Google Cloud:** Vertex AI Agent Engine, Vertex AI Gemini, Cloud Shell
* **Google Workspace:** Google Docs, Google Apps Script
* **Agent Framework:** Python, Agent Development Kit (ADK)
* **Authentication:** OAuth 2.0 (via Google Apps Script `OAuth2` library)

---

## 🚀 Project Showcase

### 1. Agent Development & Testing (ADK)

The agent's logic was built in Python and tested locally using the `adk test` command, which provides an interactive UI to validate the agent's multi-step reasoning and tool-use capabilities.

![image alt](https://github.com/balaji200528/Google-Docs-Agent-with-ADK/blob/e803a30ba4cb5936e4d01566438302361992f5ef/assets/Screenshot%202025-10-27%20154130.png)

### 2. Deployment to Vertex AI

The agent was deployed from the Google Cloud Shell to a secure, scalable endpoint on the Vertex AI Agent Engine using the `adk deploy` command.

![image alt](https://github.com/balaji200528/Google-Docs-Agent-with-ADK/blob/a3d3e24b8a8ba5b061a7d6a0dcfd7868da365ad9/assets/image.png)

### 3. Final Integration in Google Docs

The final product: a user-facing tool inside Google Docs that calls the deployed AI agent and returns a fully formatted fact-checking report.

![image alt](https://github.com/balaji200528/Google-Docs-Agent-with-ADK/blob/e803a30ba4cb5936e4d01566438302361992f5ef/assets/Screenshot%202025-10-27%20161239.png)

---

## 🔧 Setup and Deployment

This project was built based on the "Build a Google Docs fact-checking agent with Vertex AI" Codelab.

The key steps to reproduce this project are:

1.  **Set Up Cloud Environment:**
    * Enable the Vertex AI API and Google Search API in a Google Cloud project.
    * Create a dedicated Service Account with `Vertex AI User` and `Service Account User` roles.
2.  **Build the Agent (Python):**
    * Define the agent's tools (`Google Search`) and instructions in `agent.py`.
    * Use `@agent.entrypoint` and `@agent.command` decorators to define the agent's logic.
3.  **Deploy the Agent:**
    * Run `adk deploy agent_engine` from the Cloud Shell to create the Vertex AI endpoint.
4.  **Configure Google Docs Frontend:**
    * Create a new Google Apps Script project attached to a Google Doc.
    * Add the `OAuth2` library (Script ID: `1B7FSrk5Zi6L1rSxxM88EXsGoI-AF2CmcYMOiVtLIBN4VvupIYYM_0eaR`).
    * Add the Service Account JSON key, Project ID, and deployed Agent ID to the Script Properties.
    * Paste the `Code.gs` and `AIVertex.gs` code to connect the UI to the AI backend.

### Acknowledgements

* This entire project is a practical implementation of the official **[Google Codelab: Build a Google Docs fact-checking agent with Vertex AI](https://codelabs.developers.google.com/google-docs-adk-agent?hl=en)**.
