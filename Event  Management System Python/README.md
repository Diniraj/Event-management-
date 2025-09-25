# Event Management System (Flask + MongoDB)

A modern Flask application for managing events, venues, catering and vendors. Data is stored in MongoDB. UI uses Bootstrap 4.

## Features
- Authentication with roles: User, Admin, SubAdmin, SuperAdmin
- CRUD for Hotels, Catering, Vendors, Bookings
- Razorpay order/verification endpoints (optional; keys required)
- PDF receipts via xhtml2pdf
- Admin JSON import/export to/from `data/` folder

## Prerequisites
- Python 3.9+
- MongoDB (mongod) and mongosh installed and running locally

## Setup
1) Create and activate venv (Windows PowerShell)
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```
2) Install dependencies
```powershell
pip install -r requirements.txt
```
3) Run the server
```powershell
python run.py
```
Open `http://127.0.0.1:5000`.

## MongoDB
- Default URI: `mongodb://localhost:27017`
- Default DB: `event_management`
- Configure with env vars: `MONGO_URI`, `MONGO_DB_NAME`

Seed data (optional):
```powershell
mongosh --quiet --eval "db = db.getSiblingDB('event_management'); db.events.insertMany([{eventname:'Wedding'},{eventname:'Birthday Party'}])"
```

Admin user (example):
```powershell
mongosh --quiet --eval "db = db.getSiblingDB('event_management'); db.users.insertOne({email:'admin@example.com', first_name:'Admin', last_name:'User', password:'admin123', role:'Admin'})"
```

## Import/Export JSON
- Export (admin only): POST `/admin/export-json` (button in Admin Home) → writes to `data/*.json`
- Import (admin only): POST `/admin/import-json` → loads from `data/*.json` into MongoDB

## Notes
- SQLAlchemy was removed; all routes use MongoDB via PyMongo.
- Images for carousel/buffet are in `app/static/images/` (`Catering.png`, `Wedding.png`).
- For production, use a WSGI server (e.g., gunicorn + reverse proxy) and configure secrets via env vars.
