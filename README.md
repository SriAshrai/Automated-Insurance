Insurance Document Automation with Streamlit & AI
https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=Streamlit&logoColor=white
https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white
https://img.shields.io/badge/Google_Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white

Automated insurance document processing system that extracts data from PDF photo reports using OCR and AI, then fills DOCX templates with the extracted information. Built with Streamlit and deployed via Google Colab with Cloudflare Tunnel.

Key Features ✨
AI-Powered Data Extraction: Uses OpenRouter AI models to extract structured data from insurance reports

OCR Processing: Combines PyMuPDF and Tesseract for reliable text extraction from scanned documents

Dynamic Templating: Populates DOCX templates with extracted data using XML manipulation

Model Fallback System: Automatically switches to Claude Haiku if primary model fails

Regex Fallback: Robust pattern matching for when AI processing fails

Cloud Deployment: Runs entirely in Google Colab with public access via Cloudflare Tunnel

How It Works 🛠️
Upload a DOCX template and PDF photo reports

System extracts text using OCR (PyMuPDF + Tesseract fallback)

AI processes text to extract structured insurance claim data

System populates the DOCX template with extracted data

Download the completed insurance document

Installation & Setup (Google Colab) ⚙️
1. Open in Colab
https://colab.research.google.com/assets/colab-badge.svg

2. Run Initial Setup
python
# Install dependencies
!pip install -q streamlit openai PyMuPDF pdfplumber pytesseract pillow python-docx
!sudo apt install -q tesseract-ocr libtesseract-dev

# Install Cloudflare Tunnel
!wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64 -O cloudflared
!chmod +x cloudflared
!mv cloudflared /usr/local/bin/
3. Upload Your Files
Run the file upload cells to provide:

A DOCX template file

PDF photo reports

4. Launch Application
python
# Start Streamlit with Cloudflare Tunnel
import subprocess
import threading
import time

def run_streamlit():
    subprocess.run(["streamlit", "run", "app.py", "--server.port", "8501", "--server.headless", "true"])

streamlit_thread = threading.Thread(target=run_streamlit, daemon=True)
streamlit_thread.start()
time.sleep(15)

def run_cloudflared():
    subprocess.run(["/usr/local/bin/cloudflared", "tunnel", "--url", "http://localhost:8501"], 
                   stdout=open('tunnel.log', 'w'), 
                   stderr=subprocess.STDOUT)

tunnel_thread = threading.Thread(target=run_cloudflared, daemon=True)
tunnel_thread.start()
time.sleep(20)
5. Access Application
Check the output for your public Cloudflare URL (format: https://<random-subdomain>.trycloudflare.com)

Usage Guide 🚀
Enter API Key: Get your OpenRouter API key from https://openrouter.ai/keys

Select AI Model: Choose from Claude Haiku, DeepSeek, Gemini Pro, or Mistral 7B

Upload Documents:

DOCX template in left panel

PDF photo reports in right panel

Generate Document: Click the "Generate Document" button

Download Result: Get your filled insurance document

File Structure 📁
text
app.py                     # Streamlit application
modules.py                 # Core processing functions (OCR, AI, templating)
templates/                 # Directory for DOCX templates
photo_reports/             # Directory for PDF photo reports
tunnel.log                 # Cloudflare tunnel logs
Key Functions 🔍
extract_text_from_pdf(pdf_path)
Uses PyMuPDF for text extraction with Tesseract OCR fallback

Handles both text-based and image-based PDFs

extract_kv_pairs(text, api_key, model_id)
Uses AI to extract structured insurance claim data

Implements model fallback (Claude Haiku)

Includes regex fallback extraction

fill_template(template_path, context_data)
Populates DOCX templates using direct XML manipulation

Handles multiple placeholder formats

Provides default values for missing fields

Supported Data Fields 📋
Field Name	Description	Example
claim_number	Insurance claim number	CL123456
insured_name	Policyholder name	John Doe
address_of_loss	Location of incident	123 Main St
date_of_loss	Date of incident	2023-08-15
policy_number	Insurance policy number	POL987654
damage_description	General damage description	Wind damage
roof_damage_details	Specific roof damage	Missing shingles
fence_damage	Fence damage details	3 sections destroyed
pool_damage	Pool damage assessment	None
food_loss_amount	Food spoilage cost	250
mortgage_company	Mortgage lender	ABC Mortgage
Requirements 📦
requirements.txt
streamlit
openai
PyMuPDF
pdfplumber
pytesseract
pillow
python-docx
Important Notes ⚠️
Colab Limitations:

Runtime disconnects after 90 minutes of inactivity

Filesystem is temporary (save downloaded files immediately)

Keep Colab tab open to maintain connection

Cloudflare Tunnel:

Free public URL (no authentication)

Tunnel automatically terminates when Colab session ends

URL changes with each new session

AI Processing:

Requires OpenRouter API key

First model attempt falls back to Claude Haiku if needed

Regex extraction used as final fallback

Troubleshooting 🔧
Missing Text Extraction: Ensure PDFs contain clear text/images

AI Processing Errors: Verify API key and check model availability

Connection Issues: Restart runtime and rerun all cells

Template Not Filling: Use exact placeholder format {{field_name}}

License 📄
MIT License - See LICENSE for details
