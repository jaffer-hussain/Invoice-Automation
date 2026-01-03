📄 Invoice Automation Pipeline (PDF & Image → Google Sheets)

An end-to-end invoice processing automation built using n8n, OCR, and LLMs to extract structured data from PDF and image invoices and store it in Google Sheets.

This project handles real-world challenges such as mixed file types, OCR failures, rate limits, and inconsistent document formats.

🚀 Features

✅ Supports PDF and image (PNG/JPG) invoices

✅ Works with scanned & digital PDFs

✅ Extracts structured invoice data using OCR + LLM

✅ Automatically stores data in Google Sheets

✅ Handles rate limits and partial OCR failures

✅ Prevents silent data drops using validation logic

🛠️ Tech Stack

n8n – Workflow orchestration

OCR.space API – Optical Character Recognition

LLM (Gemini / OpenAI) – Information extraction

Google Drive – Invoice source

Google Sheets – Final structured output

🧠 Workflow Architecture
Google Drive (Search files)
 → Loop Over Items (batch = 1)
 → Download File
 → Move file to Base64 string
 → OCR (PDF + PNG)
 → IF (OCR text exists)
 → Wait (rate-limit handling)
 → Information Extractor (LLM + JSON Schema)
 → Split line items
 → Append data to Google Sheets

🔍 OCR Strategy

All files (PDF & images) are processed only through OCR to ensure a consistent output format.

OCR Input Handling
{{ $json.data.startsWith("JVBER")
  ? "data:application/pdf;base64," + $json.data
  : "data:image/png;base64," + $json.data }}


This ensures:

PDFs are treated as application/pdf

Images are treated as image/png

✅ OCR Validation Logic

Before sending data to the LLM, OCR results are validated:

{{$json.ParsedResults && $json.ParsedResults.length > 0}}


Only items with extracted text proceed further, preventing runtime errors like:

“Text for item 0 is not defined”

🧩 Information Extracted
Invoice Header

Invoice number

Invoice date

Billing company

Billing address

Billing email

Customer name

Subtotal

Tax

Total amount

Invoice Line Items

Item name

Quantity

Unit price

Line total

📊 Output Structure
Google Sheets

Sheet 1: Invoice headers

Sheet 2: Invoice line items

Each invoice and its items are stored in a normalized, analytics-ready format.

⚠️ Challenges Solved

Mixed file types (PDF + PNG)

OCR failures without crashing the workflow

LLM rate-limit handling using batching and wait nodes

Base64 encoding issues (filesystem-v2 vs real base64)

Preventing silent drops in IF nodes

Schema consistency between PDFs and images

🧪 How to Run

Upload invoice files (PDF/PNG) to Google Drive

Configure API keys (OCR + LLM)

Run the n8n workflow

View extracted data in Google Sheets

📌 Key Learnings

OCR output must be normalized before LLM processing

PDFs often behave differently from images

IF nodes can silently filter items if not handled carefully

Batch size = 1 + wait nodes = reliable rate-limit handling

Production automation requires defensive validation at every step

📈 Future Improvements

Add retry logic for failed OCR files

Improve multi-page PDF handling

Add confidence scoring for extracted fields

Switch to higher-accuracy OCR engines (Azure / Google Vision)

Export data to databases (PostgreSQL / BigQuery)

👤 Author

Jaffer Hussain
Data Analyst | Power BI Developer | Automation Enthusiast

🔗 LinkedIn: https://www.linkedin.com/in/jaffer-hussain-dataanalyst-powerbideveloper/

🔗 GitHub: https://github.com/jaffer-hussain
