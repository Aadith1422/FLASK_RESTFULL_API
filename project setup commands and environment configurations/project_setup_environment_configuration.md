# Project Setup and Environment Configuration

## 1. Project Initialization

The project was initialized by creating a dedicated directory for the Flask REST API.

### Command Used
```bash
mkdir flask_rest_api_project
cd flask_rest_api_project
```

---

## 2. Creating a Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate
```

After activation:
```
(venv) user@system:~/flask_rest_api_project$
```

---

## 3. Installing Required Dependencies

```bash
pip install flask flask-restx flask-jwt-extended flask-sqlalchemy flask-migrate marshmallow bcrypt python-dotenv
```

---

## Libraries Used

| Library | Purpose |
|--------|--------|
| Flask | Core web framework |
| Flask-RESTX | API structuring and documentation |
| Flask-JWT-Extended | Authentication using JWT |
| Flask-SQLAlchemy | Database ORM |
| Flask-Migrate | Database migrations |
| Marshmallow | Input validation |
| bcrypt | Password hashing |
| python-dotenv | Environment variable management |

---

## 4. Saving Dependencies

```bash
pip freeze > requirements.txt
```

---

## 5. Project Folder Structure

```bash
mkdir routes models utils
touch app.py config.py extensions.py
touch routes/auth_routes.py
touch routes/user_routes.py
touch models/user_model.py
touch utils/security.py
```

---

## Project Structure

```
flask_rest_api_project
│
├── app.py
├── config.py
├── extensions.py        
├── requirements.txt
│
├── routes
│   ├── auth_routes.py
│   └── user_routes.py
│
├── models
│   └── user_model.py
│
└── utils
    └── security.py
```

---

## 6. Environment Configuration

```python
import os

class Config:
    SECRET_KEY = os.getenv("SECRET_KEY", "supersecretkey")
    JWT_SECRET_KEY = os.getenv("JWT_SECRET_KEY", "jwt-secret")
    SQLALCHEMY_DATABASE_URI = os.getenv("DATABASE_URL", "sqlite:///users.db")
    SQLALCHEMY_TRACK_MODIFICATIONS = False
```

---

## 7. Database Initialization

```bash
export FLASK_APP=app.py
flask db init
flask db migrate -m "Initial migration"
flask db upgrade
```

---

## 8. Running the Application

```bash
python app.py
```

Access:
```
http://127.0.0.1:5000
```

---

## 9. Version Control

```bash
git init
git add .
git commit -m "Initial Flask REST API setup with JWT and DB"
```

---

## 10. Security Features Implemented

- JWT Authentication (Access + Refresh Tokens)
- Password hashing using bcrypt
- Input validation
- Role-Based Access Control (RBAC)
- Security headers

---
