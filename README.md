# AI News Podcast Producer

An automated multi-agent system that researches the latest AI news for NASDAQ-listed U.S. companies, enriches findings with financial data, generates a structured Markdown report, converts the report into a conversational podcast script, and finally produces a multi-speaker audio podcast using Google’s Gemini TTS APIs.

---

## ⭐ Key Features

- **AI-powered news discovery** using Google Search (with domain whitelisting + freshness filters).
- **Structured news extraction** using Pydantic models (`NewsStory`, `AINewsReport`).
- **Integrated financial enrichment** with real-time stock data from Yahoo Finance.
- **Automated Markdown report generation** (saved as `ai_research_report.md`).
- **Podcast script creation** between two hosts, *Joe* and *Jane*.
- **Multi-speaker podcast audio generation** via Gemini TTS (`gemini-2.5-flash-preview-tts`).
- **Multi-agent workflow** (Root Agent + Podcaster Agent) powered by Google ADK.
- **Callback-enhanced tool behavior** for search whitelisting, freshness enforcement, and process logging.

---

## 📁 Project Structure

- **Agents**
  - `root_agent` — orchestrates research, reporting, and podcast creation.
  - `podcaster_agent` — converts a script to audio using Gemini TTS.

- **Tools**
  - `google_search` — searches for AI news from whitelisted domains.
  - `get_financial_context` — retrieves current prices and percent changes with `yfinance`.
  - `save_news_to_markdown` — saves the structured report as Markdown.
  - `generate_podcast_audio` — synthesizes WAV audio from the conversational script.

- **Schemas**
  - `NewsStory`
  - `AINewsReport`

- **Callbacks**
  - Domain whitelist enforcement.
  - Freshness filtering (`tbs=qdr:w`).
  - Process log collection and injection.

---

## 🧠 How It Works (Workflow Overview)

1. **User triggers research**  
   The Root Agent acknowledges the request and begins silent background processing.

2. **Search for AI news**
   - Calls `google_search`
   - Domain whitelist applied
   - Results limited to past week  
   - Process logs added automatically

3. **Extract companies & tickers**  
   Unknown or non-NASDAQ tickers become `"N/A"`.

4. **Fetch financial data**
   - Uses `yfinance`
   - Missing data becomes `"Not Available"`

5. **Create structured report**  
   Using the `AINewsReport` schema, including full `process_log`.

6. **Generate Markdown report**  
   Written to `ai_research_report.md`, with a final section:  
   **“## Data Sourcing Notes”**

7. **Create conversational podcast script**  
   Between **Joe** (enthusiastic) and **Jane** (analytical).

8. **Generate podcast audio**  
   Sent to the secondary `podcaster_agent`, which uses Gemini TTS.  
   Produces a `.wav` file.

9. **Final confirmation**  
   The Root Agent completes the workflow and notifies the user.

