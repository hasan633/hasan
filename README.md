# Gemini Chat — Vercel

Chat only. No website analyzer, product information, catalog scans or e-commerce controls.

## Deploy

1. Extract the ZIP and upload its contents to your GitHub repository root. Keep `index.html`, `api/chat.js`, `package.json` and `vercel.json` in the shown structure. Do not upload only the HTML file.
2. Import the repository in Vercel. Framework: **Other**. Root directory: repository root. No build command. Output directory: `.` if requested.
3. Set these Vercel Environment Variables:

| Name | Value |
|---|---|
| GEMINI_API_KEY | Your Google AI Studio API key |
| AI_ACCESS_PASSWORD | A long unique chat password you choose |
| GEMINI_MODEL | gemini-2.5-flash-lite |

4. Deploy (or redeploy after adding/changing variables).
5. Open the deployed URL, click **Chat access**, and enter your chat password. Ask your question.

Get your API key: https://aistudio.google.com/apikey
Never put the real API key in HTML or GitHub. Environment variables are read only by `api/chat.js` on the server. `.env.example` contains placeholders only.

## Features

- Responsive chat interface with suggestions and safe code-block display
- New conversations and in-tab conversation switching
- Follow-up questions using the last eight messages (each capped at 8,000 characters)
- Stop generation, copy response, access-password dialog
- Quota, timeout, model and configuration error messages
- HTML, CSS and frontend JavaScript combined in `index.html`

Chats and the access password are held in memory only. Refreshing clears them. The mobile interface shows the active conversation and a New button; the desktop sidebar lists conversations from this tab.

Gemini receives questions and recent chat history. No browsing, search, attachments or external action tools are enabled. Responses can be incorrect. Free-tier data may be used by Google to improve products; avoid confidential material.

Free-tier availability and quotas depend on your Google project and model. The app does not enable billing or switch to a paid model, but cannot enforce free pricing for a billing-enabled Google project. Verify your account's quota at https://ai.google.dev/gemini-api/docs/pricing. If the model is unavailable, change GEMINI_MODEL to an available text model and redeploy. Vercel hosting limits are separate.

The shared password restricts access; it is not a multi-user authentication or distributed rate-limiting system. Only share it with trusted users who may use your quota.

## Test locally

Node.js 22 or later: `npm test`.
For the full app, use `vercel dev` with values in an uncommitted `.env.local`. Opening index.html directly cannot run the backend.

Tests use mocked Gemini responses; a live Gemini call requires your API key. This package is prepared for deployment but has not been deployed to your Vercel account.
