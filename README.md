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
