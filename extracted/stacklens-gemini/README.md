# StackLens + Gemini

A website technology and e-commerce analyzer with a Gemini question panel. Frontend HTML, CSS and JavaScript stay in one `index.html`; the small `api/chat.js` Vercel Function keeps the Gemini API key on the server.

## Deploy from GitHub to Vercel

1. Extract this ZIP. Upload the **contents of `stacklens-gemini/`** to your GitHub repository root. The root must contain `index.html`, `api/`, `package.json` and `vercel.json`. Do not upload just the ZIP or just the HTML.
2. Create a Gemini API key in [Google AI Studio](https://aistudio.google.com/apikey). Use a project with free-tier access and available quota.
3. In Vercel, choose **Add New → Project** and import that GitHub repository. Choose **Other** as the framework. Use the repository root as Root Directory, no build command, and `.` as Output Directory if Vercel requests one. There are no package dependencies to install.
4. Before deploying, add these server environment variables for Production (and Preview if desired):

   | Variable | Value |
   | --- | --- |
   | `GEMINI_API_KEY` | Your Google AI Studio API key |
   | `AI_ACCESS_PASSWORD` | A long, unique password you choose for this app's chat |
   | `GEMINI_MODEL` | `gemini-2.5-flash-lite` (default; change if unavailable for your project) |

5. Deploy. If variables are added or changed afterward, redeploy for them to take effect.
6. Open the Vercel URL, analyze a website, and enter your **chat access password** in the Gemini panel. Ask a question. The chat password is not the Gemini API key.

Do not put your actual API key in HTML, GitHub, `.env.example`, or the chat input. `.gitignore` excludes real local environment files. If the browser exposes your API key, the integration has been configured incorrectly.

## What you can ask

- Which platform, theme, apps and plugins were detected?
- What does each detected integration do?
- Which findings are confirmed signals versus inferences?
- Summarize the products, collections and pages found.
- Which SEO checks should I prioritize based on the available evidence?
- What does PHP or Laravel mean for this website?

The checkbox **Include current scan results** controls whether scan data is attached. Uncheck it to ask general questions. Suggested questions fill the input; they do not send automatically. Clear chat removes the in-memory conversation. A new scan resets the conversation to avoid mixing websites.

## Free tier and privacy

Google currently lists free-tier access for Gemini 2.5 Flash-Lite. Availability and quotas depend on the model, region and project; free does not mean unlimited. The app does not enable billing, change your Google plan, or retry on a paid model. It also cannot enforce a free-only billing tier: if you use a billing-enabled project, requests follow that project's pricing. For a zero-cost setup, use a free-tier project with available quota. Vercel hosting limits and eligibility are separate.

Questions, up to eight recent chat messages and selected report data are sent to Google only when you click **Ask Gemini**. Google's free-tier pricing documentation indicates data may be used to improve its products. Do not submit confidential information. Scan reports sent to Gemini omit raw source, cookies, response headers and the API key. The access password goes only to this app's server in an HTTPS request header and is not included in Gemini prompts or saved in browser storage.

References checked for this implementation:
- [Gemini pricing and free-tier terms](https://ai.google.dev/gemini-api/docs/pricing)
- [Gemini API keys](https://ai.google.dev/gemini-api/docs/api-key)
- [Gemini REST API](https://ai.google.dev/api)
- [Vercel Node.js Functions](https://vercel.com/docs/functions/runtimes/node-js)

## Coverage and limitations

- Website fetching retains the original direct, public AllOrigins proxy and custom-proxy options. This package adds the Gemini backend; it does **not** add a server-side crawler. Proxy availability, CORS and anti-bot restrictions still affect scans.
- Gemini reasons about provided data; it does not browse, execute website code or find additional hidden apps. Its advice can be wrong. Public technology signals are not proof of activation.
- Product, collection and page counts are public URLs found, not administrative totals. Partial scan warnings remain visible and are sent to Gemini.
- For larger catalogs, chat includes counts and up to 40 question-relevant items per selected section. The sample is labeled. Full inventories remain available through the analyzer's CSV/JSON exports. AI cannot list all products when only a sample was supplied.
- A shared password protects the API from unauthenticated use; it is not a multi-user login system or distributed rate limiter. If you share the password, those users share your Gemini quota. For a broadly public app, add per-user authentication and durable rate limits before distributing access.
- Demo results are synthetic and labeled. Questions about them use demo data.
- Opening `index.html` as a local file still runs offline analysis, but Gemini requires the deployed `/api/chat` function.

## Local development and tests

Requires Node.js 22 or newer. Run `npm test` for the server tests. They mock Gemini and cover configuration, access control, validation, report sampling, provider failures and quota handling without a real API key.

For local full-stack development, install/use the Vercel CLI, copy `.env.example` to `.env.local`, insert real values locally, and run `vercel dev`. Do not commit `.env.local`.

The integration was checked with mocked Gemini responses and DOM-based frontend tests. A real Gemini call was not made because no API key was supplied. Deployment and your account's model access remain to be verified after setting environment variables.

## Troubleshooting

- **Chat is not configured:** Set both required variables in the correct Vercel environment and redeploy.
- **Incorrect chat access password:** Enter the value of `AI_ACCESS_PASSWORD`, not the Google API key.
- **404 / model unavailable:** Select an available free-tier text model in Google AI Studio and update `GEMINI_MODEL`.
- **Quota / 429:** Check your project's limits or retry later. The app never silently switches to a paid model.
- **Gemini rejected configuration:** Check key permissions, model access and supported region in Google AI Studio.
- **API response is HTML / endpoint missing:** Ensure `api/chat.js` was uploaded at the repository root and deployed with the project.
- **Website scan blocked:** Use the offline HTML option or a correctly configured custom proxy. Chat cannot recover data the scanner could not access.
