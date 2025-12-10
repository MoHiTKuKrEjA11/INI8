# Patient Portal

## Project Overview

The Patient Portal is a full-stack web application that allows patients to upload, view, download, and delete PDF medical documents such as prescriptions, test results, and referral notes.

**Tech Stack:**

- **Frontend:** React.js (TypeScript)
- **Backend:** Flask (Python)
- **Database:** PostgreSQL
- **File Storage:** Local `uploads/` folder

---

## Running Locally

### Backend Setup

```bash
cd patient-portal-be
python3 -m venv venv

# Activate virtual environment
# Windows (PowerShell)
.\venv\Scripts\Activate.ps1
# Linux/Mac
source venv/bin/activate

pip install -r requirements.txt

# Optional: Create .env file with your database URL
# DATABASE_URL=postgresql://username:password@host:port/databasename

python app.py

Backend runs at http://localhost:5000
```

### Frontend Setup

```bash
cd patient-portal-fe
npm install
npm run dev

Frontend runs at http://localhost:5173
```

### API Calls (Using curl)

1: Upload a PDF

```bash
curl -X POST http://localhost:5000/documents/upload \
  -F "file=@/path/to/file.pdf"

```

2: List Documents

```bash
curl http://localhost:5000/documents

```

3: Download Document

```bash
curl -O http://localhost:5000/documents/1

```

4: Delete Document

```bash
curl -X DELETE http://localhost:5000/documents/1

```
