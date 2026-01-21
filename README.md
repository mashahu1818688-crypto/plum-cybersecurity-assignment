# plum-cybersecurity-assignment
Cyber Security Pre-screening Assignment – Plum
from fastapi import FastAPI, UploadFile, File, HTTPException
from pydantic import BaseModel
import pytz
import dateparser
import uvicorn

app = FastAPI()

class TextRequest(BaseModel):
    text: str

@app.post("/appointment/text")
async def process_text(request: TextRequest):
    raw_text = request.text
    confidence = 0.90  # mock confidence

    # Step 1: OCR/Text extraction (for text input, just echo)
    # Step 2: Entity Extraction (simple demo)
    # Here, just simple keyword extraction for example:
    department = None
    date_phrase = None
    time_phrase = None

    text_lower = raw_text.lower()

    # Extract department keywords (simple example)
    if "dentist" in text_lower:
        department = "Dentist"
    elif "doctor" in text_lower:
        department = "Doctor"

    # Extract date and time using dateparser (basic)
    settings = {'TIMEZONE': 'Asia/Kolkata', 'RETURN_AS_TIMEZONE_AWARE': True}

    date_time = dateparser.parse(raw_text, settings=settings)
    if date_time:
        date_iso = date_time.strftime("%Y-%m-%d")
        time_iso = date_time.strftime("%H:%M")
    else:
        return {"status": "needs_clarification", "message": "Ambiguous date/time or department"}

    # Simple guardrail
    if not department:
        return {"status": "needs_clarification", "message": "Ambiguous date/time or department"}

    return {
        "appointment": {
            "department": department,
            "date": date_iso,
            "time": time_iso,
            "tz": "Asia/Kolkata"
        },
        "status": "ok"
    }

if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8000)
    

# Project Title  
## Overview  
### Features  
## Setup Instructions  
## API Usage  
## Author

# AI-Powered Appointment Scheduler Assistant

## Overview
This backend service parses appointment requests and returns structured scheduling data.

## Setup Instructions
...

## API Usage
...

## Author
Mohamed Mashahu M

git add README.md
git commit -m "Update README with project headings and description"
git push

 create README.md with headings.


 

sudo apt-get install tesseract-ocr

brew install tesseract


pip install pytesseract pillow

pytesseract
pillow
from fastapi import File, UploadFile
from PIL import Image
import pytesseract
import io

@app.post("/appointment/image")
async def process_image(file: UploadFile = File(...)):
    try:
        contents = await file.read()
        image = Image.open(io.BytesIO(contents))
        raw_text = pytesseract.image_to_string(image)

        confidence = 0.85  # Mock confidence for example

        # You can reuse your text processing logic here:
        # For simplicity, call your existing process_text logic
        # But since it's async, let's simulate it:

        # Return raw_text and confidence for now
        return {
            "raw_text": raw_text.strip(),
            "confidence": confidence
        }
    except Exception as e:
        return {"status": "error", "message": str(e)}

 http://127.0.0.1:8000/appointment/image



 appointment/image
@app.post("/appointment/image")
async def process_image(file: UploadFile = File(...)):
    try:
        contents = await file.read()
        image = Image.open(io.BytesIO(contents))
        raw_text = pytesseract.image_to_string(image).strip()

        confidence = 0.85  # Mock confidence

        # Reuse logic from /appointment/text endpoint
        text_lower = raw_text.lower()

        department = None
        if "dentist" in text_lower:
            department = "Dentist"
        elif "doctor" in text_lower:
            department = "Doctor"

        settings = {'TIMEZONE': 'Asia/Kolkata', 'RETURN_AS_TIMEZONE_AWARE': True}
        date_time = dateparser.parse(raw_text, settings=settings)

        if not department or not date_time:
            return {"status": "needs_clarification", "message": "Ambiguous date/time or department"}

        date_iso = date_time.strftime("%Y-%m-%d")
        time_iso = date_time.strftime("%H:%M")

        return {
            "appointment": {
                "department": department,
                "date": date_iso,
                "time": time_iso,
                "tz": "Asia/Kolkata"
            },
            "status": "ok"
        }

    except Exception as e:
        return {"status": "error", "message": str(e)}

Friday at 3pm

Book dentist next Friday at 3pm

{
  "appointment": {
    "department": "Dentist",
    "date": "2025-09-26",
    "time": "15:00",
    "tz": "Asia/Kolkata"
  },
  "status": "ok"
}

appointment/text

curl -X POST "http://127.0.0.1:8000/appointment/text" \
-H "Content-Type: application/json" \
-d '{"text":"Book dentist next Friday at 3pm"}'

curl -X POST "http://127.0.0.1:8000/appointment/image" \
-H "Content-Type: multipart/form-data" \
-F "file=@appointment.jpg"



## API Usage Examples

### Text Input Example

```bash
curl -X POST "http://127.0.0.1:8000/appointment/text" \
-H "Content-Type: application/json" \
-d '{"text":"Book dentist next Friday at 3pm"}'

curl -X POST "http://127.0.0.1:8000/appointment/image" \
-H "Content-Type: multipart/form-data" \
-F "file=@appointment.jpg"

---

### Step: Add these to README.md, then commit & push

```bash
git add README.md
git commit -m "Add sample curl requests to README"
git push

Sample API requests added to README and pushed


requirements.txt

fastapi
uvicorn
dateparser
pytz
pytesseract
pillow

git add requirements.txt
git commit -m "Add requirements.txt with project dependencies"
git push

requirements.txt added and pushed

cd path/to/appointment-scheduler

python -m venv venv
venv\Scripts\activate

python3 -m venv venv
source venv/bin/activate

pip install -r requirements.txt
uvicorn main:app --reload

http://127.0.0.1:8000/docs
Backend running locally and tested



ngrok http 8000


Forwarding                    https://abcdef1234.ngrok.io -> http://localhost:8000


https://abcdef1234.ngrok.io/appointment/text

Ngrok setup done with public URL: <your-ngrok-URL>


        
 
