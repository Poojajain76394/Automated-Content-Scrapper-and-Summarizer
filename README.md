# 🚀 Automated YouTube Content Research & Summarizer Engine

An automated content research pipeline built using **n8n**, **Google Sheets**, and **Gemini AI**. This workflow automatically takes target keywords submitted via a Google Form, scrapes YouTube video data, processes structured insights using Gemini AI, saves the organized content ideas into a master Google Sheet, and updates the task status to prevent redundant processing.

---

## 📌 Architecture & Workflow Summary

```text
[Google Form] 
     │ (User submits keyword)
     ▼
[Google Sheet: Form Responses] 
     │ (Triggers workflow where Status is Empty)
     ▼
[n8n Workflow Execution]
     │
     ├── 1. Deduplication (Removes repeated input keywords)
     ├── 2. HTTP Request (Fetches YouTube API search results - Top 5 videos)
     ├── 3. Split Out / Itemization (Processes each video item)
     ├── 4. AI Agent (Gemini AI + Structured Output Parser)
     │       ├── Video Summary
     │       ├── Content & Hook Ideas
     │       └── Audience Pain Points
     │
     ├── 5. Append Row (Saves AI output to Target Google Sheet)
     └── 6. Update Row (Marks 'Status = Done' in Source Form Responses Sheet)
🛠️ How It Works (Step-by-Step)
​Step 1: User Input via Google Form
​The user inputs a target topic or keyword into a custom Google Form (e.g., n8n, AI Automation).
​The response is automatically recorded as a new row in the primary Google Sheet (Form Responses).
​Step 2: Triggering & Filtering
​n8n Trigger: Detects new entries or reads rows from the Form Responses Google Sheet where the Status column is blank (empty).
​Deduplication Node: Cleans input batch data by running Remove items repeated within current input, ensuring duplicate keywords within the same execution cycle are filtered out.
​Step 3: YouTube Data Scraping
​An HTTP Request node sends a query to the YouTube API/Scraper endpoint using the keyword.
​For each keyword, it fetches top video metadata including:
​Video Title
​Channel Name
​Video URL
​Video Description / Content details
​Step 4: AI Analysis via Gemini AI Agent
​The items are passed to an AI Agent Node powered by Google Gemini Chat Model and a Structured Output Parser.
​Gemini processes each video's details and generates:
​High-level Summary
​Actionable Content Ideas
​Key Audience Pain Points & Keywords
​Step 5: Data Storage (Target Google Sheet)
​An Append Row (Google Sheets) node receives the structured AI response.
​It creates a new record for every analyzed video in a separate Target Output Sheet (content ideas), populating columns like Title, Channel, Video URL, Content Ideas, Keywords, and Audience Pain Points.
​Step 6: Status Update (Source Google Sheet)
​Once the output is successfully saved, an Update Row (Google Sheets) node locates the original keyword row in the source sheet (using row_number or Keyword as match criteria).
​It updates the Status column value to Done, preventing the workflow from re-processing the same topic on future runs.
​📋 Prerequisites
​n8n Instance (Self-hosted or Cloud)
​Google Account (Google Sheets & Google Forms access)
​Google Cloud Console Credentials / OAuth2 (For Google Sheets API)
​Gemini API Key (Configured in n8n AI node)
​YouTube Search API Key (Or custom scraping endpoint credentials)
​⚙️ Setup Instructions
​Create the Form & Sheets:
​Create a Google Form asking for Enter your keyword.
​Open the linked response sheet and manually add a column named Status.
​Create a Destination Google Sheet with columns: Title, Channel, video URL, Content ideas, Keywords, Audience Pain Points.
​Import Workflow in n8n:
​Copy the workflow JSON or recreate the node connections as per the architecture diagram.
​Configure Credentials:
​Connect your Google OAuth2 credentials to both Google Sheets nodes.
​Add your Gemini API credential to the Gemini Chat Model sub-node.
​Run & Test:
​Fill out the Google Form.
​Click Execute Workflow in n8n to test end-to-end execution.
​🎯 Key Benefits
​Zero Duplication: Smart filtering prevents re-scraping existing queries.
​Structured Data: AI Output Parsers guarantee neat formatting in spreadsheets.
​Fully Automated: Takes less than 2 minutes to transform raw keywords into structured content strategy.
