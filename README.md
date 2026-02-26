# Society Account Management Website (Flask + SQL)

A dynamic web application for housing society accounting and administration.

## Features

### Core modules
- Member registration and JWT-based login
- Member profile and role management (Admin, Treasurer, Member)
- Maintenance payment records (online/offline)
- Expense tracking with bill upload
- Income vs expense dashboard with charts
- Notices and announcements
- Audit logs for critical operations
- Monthly financial reports
- Auto-calculated pending dues per member
- CSV export for financial data

### Access control
- **Admin**: Full control (members CRUD, payments, expenses, reports, notices)
- **Treasurer**: Financial operations (payments, expenses, reports, notices)
- **Member**: View own profile, own payment history, and notices

---

## Project structure

```
/workspace/society
├── app/
│   ├── __init__.py
│   ├── models.py
│   ├── routes.py
│   ├── static/
│   │   ├── css/style.css
│   │   └── js/
│   │       ├── main.js
│   │       └── dashboard.js
│   └── templates/
│       ├── index.html
│       └── dashboard.html
├── app.py
├── requirements.txt
└── README.md
```

---

## Database schema

### `users`
- id (PK)
- name
- email (unique)
- phone
- role (`admin`, `treasurer`, `member`)
- flat_number
- password_hash
- monthly_fee
- created_at

### `payments`
- id (PK)
- user_id (FK -> users.id)
- amount
- payment_date
- payment_mode
- receipt_number (unique)
- status
- created_at

### `expenses`
- id (PK)
- category
- amount
- description
- date
- bill_upload
- created_at

### `notices`
- id (PK)
- title
- content
- created_at

### `audit_logs`
- id (PK)
- user_id (nullable FK -> users.id)
- action
- created_at

---

## Setup instructions

1. **Create virtual environment**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Set environment variables (optional but recommended)**
   ```bash
   export FLASK_APP=app.py
   export SECRET_KEY='change-this-secret'
   export JWT_SECRET_KEY='change-this-jwt-secret'
   export DATABASE_URL='sqlite:///society.db'
   ```

4. **Run the app**
   ```bash
   flask run
   ```

5. Open in browser: `http://127.0.0.1:5000`

---

## Deployment steps

### Option A: Gunicorn + Nginx (VM)

1. Install dependencies and gunicorn:
   ```bash
   pip install gunicorn
   ```
2. Start app:
   ```bash
   gunicorn -w 4 -b 0.0.0.0:8000 app:app
   ```
3. Configure Nginx reverse proxy to `localhost:8000`.
4. Use HTTPS via Let's Encrypt.
5. Set production env vars for secrets and database URL.

### Option B: Docker (example)

- Build image with Python runtime and requirements.
- Run with persistent volume for SQLite file/uploads or switch to PostgreSQL.
- Set env vars for secrets.

---

## Compliance considerations (India)

- Maintain audit-ready logs and expense bills/invoices.
- Ensure personal member data is protected (restricted access + strong secrets).
- Keep payment records immutable after settlement policy.
- Use secure payment gateway integration for production online payments.
- Retain documents for statutory audit timelines under local laws.

---

## Notes

- SQLite is used by default for quick setup. For production, prefer PostgreSQL.
- JWT tokens are stored in browser localStorage in this starter; consider secure HTTP-only cookies in production.
