# Patient Portal Design Document

## 1. Tech Stack Choices

**Q1. Frontend framework**

- React.js
- Why: Allows building interactive and responsive UI. Components and state management make it easy to update the UI, e.g., showing uploaded files and refreshing the list after upload or delete.

**Q2. Backend framework**

- Flask (Python)
- Why: Lightweight, easy to set up, and works well for small projects. Allows quickly creating REST APIs for uploading, listing, downloading, and deleting files.

**Q3. Database**

- PostgreSQL
- Why: Reliable relational database suitable for structured data like filenames, file size, and timestamps. Easy to connect with Flask using SQLAlchemy.

**Q4. Scaling for 1,000 users**

- Use cloud-hosted PostgreSQL (Render, AWS RDS, etc.)
- Store files in cloud storage (AWS S3) instead of local disk
- Implement user authentication
- Use pagination for document lists
- Deploy backend with WSGI server (Gunicorn) behind load balancer

---

## 2. Architecture Overview

**Flow between frontend, backend, database, and file storage:**

- Frontend (React) handles:
  - File input
  - Displaying uploaded documents
  - Download and delete actions
- Backend (Flask) handles:
  - REST API endpoints
  - File validation and saving to `uploads/` folder
  - Metadata storage in PostgreSQL
- Database (PostgreSQL) stores:
  - id, filename, filepath, filesize, created_at
- File storage:
  - Local `uploads/` folder stores actual PDF files

---

## 3. API Specification

| Endpoint            | Method | Description                 | Sample Request              | Sample Response                                                                              |
| ------------------- | ------ | --------------------------- | --------------------------- | -------------------------------------------------------------------------------------------- |
| `/documents/upload` | POST   | Upload a PDF file           | `FormData: file=<file.pdf>` | `{ "id": 1, "filename": "file.pdf", "filesize": 1024, "created_at": "2025-12-10T12:00:00" }` |
| `/documents`        | GET    | List all uploaded documents | None                        | `[{"id":1,"filename":"file.pdf","filesize":1024,"created_at":"2025-12-10T12:00:00"}]`        |
| `/documents/:id`    | GET    | Download a document by ID   | None                        | Returns PDF file as attachment                                                               |
| `/documents/:id`    | DELETE | Delete a document by ID     | None                        | `{ "message": "Document deleted" }`                                                          |

---

## 4. Data Flow Description

**File Upload:**

- User selects a PDF in frontend
- React sends POST request to `/documents/upload` with `FormData`
- Backend validates file, generates unique filename, saves file to `uploads/`
- Metadata stored in PostgreSQL
- Backend responds with file metadata
- Frontend shows alert and refreshes document list

**File Download:**

- User clicks **Download** in frontend
- React sends GET request to `/documents/:id`
- Backend locates file in `uploads/` and returns it as attachment
- Browser downloads file with original filename

---

## 5. Assumptions

- Only PDF files allowed.
- Maximum file size: 16 MB.
- No authentication implemented (all users share same database).
- No concurrent editing/locking needed.
- Local file storage used for simplicity; cloud storage can be used in production.
