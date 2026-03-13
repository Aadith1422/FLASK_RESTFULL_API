# Project Setup and Environment Configuration

## 1. Project Initialization

The project was initialized by creating a new directory for the REST API
project.

### Command Used

``` bash
mkdir flask_rest_api_project
cd flask_rest_api_project
```

------------------------------------------------------------------------

## 2. Creating a Virtual Environment

``` bash
python3 -m venv venv
source venv/bin/activate
```

After activation the terminal prompt shows:

    (venv) user@system:~/flask_rest_api_project$

------------------------------------------------------------------------

## 3. Installing Required Dependencies

``` bash
pip install flask flask-restx flask-jwt-extended marshmallow bcrypt python-dotenv
```

### Libraries Used

  Library              Purpose
  -------------------- ------------------------------------------
  Flask                Web framework used to build the REST API
  Flask-RESTX          Helps structure REST APIs
  Flask-JWT-Extended   Implements JWT authentication
  Marshmallow          Data validation
  bcrypt               Password hashing
  python-dotenv        Environment variables

------------------------------------------------------------------------

## 4. Saving Dependencies

``` bash
pip freeze > requirements.txt
```

------------------------------------------------------------------------

## 5. Project Folder Structure

``` bash
mkdir routes models utils
touch app.py config.py
touch routes/auth_routes.py
touch routes/user_routes.py
touch models/user_model.py
touch utils/security.py
```

### Project Structure

    flask_rest_api_project
    │
    ├── app.py
    ├── config.py
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

------------------------------------------------------------------------

## 6. Environment Configuration

Example `config.py`

``` python
import os

class Config:
    SECRET_KEY = os.getenv("SECRET_KEY", "supersecretkey")
    JWT_SECRET_KEY = os.getenv("JWT_SECRET_KEY", "jwt-secret")
```

------------------------------------------------------------------------

## 7. Running the Application

``` bash
python app.py
```

Access:

    http://127.0.0.1:5000

------------------------------------------------------------------------

## 8. Version Control

``` bash
git init
git add .
git commit -m "Initial Flask REST API setup"
```
