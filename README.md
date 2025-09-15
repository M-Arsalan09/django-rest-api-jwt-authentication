# Django REST API with JWT Authentication

A minimal Django REST Framework API implementing **JWT Authentication** using [SimpleJWT](https://django-rest-framework-simplejwt.readthedocs.io/).  
Supports **user registration**, **login (access & refresh tokens)**, **logout (token blacklist)** and uses **SQLite** by default for simplicity.  
Includes a **Dockerfile** and **docker-compose.yml** for easy setup.

---

## ✨ Features
- User registration (signup)
- Login (obtain access & refresh tokens)
- Refresh token endpoint
- Logout (blacklist refresh token)
- SQLite database by default (no extra DB service required)
- Ready to run with Docker Compose
- Environment variables managed via `.env`

---

## 🛠️ Tech Stack
- Python 3.11
- Django 4.x
- Django REST Framework
- djangorestframework-simplejwt
- SQLite (default)  
- Docker / Docker Compose (optional)

---

## 📂 Project Structure
```

django-jwt-auth/
│
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── .env.example
├── manage.py
├── django\_jwt\_auth/
│   ├── settings.py
│   ├── urls.py
│   └── ...
└── users/
├── serializers.py
├── views.py
├── urls.py
└── ...

````

---

## ⚙️ Environment Variables

Copy `.env.example` to `.env` and adjust as needed:

```bash
cp .env.example .env
````

**Required variables:**

```env
SECRET_KEY="django-insecure-ime#*e3!3zz&l^3+(9r8m(fq=dwdx8mtr8im$$_+jk$0oa=c)w"
ENVIRONMENT="Staging"
```

* `SECRET_KEY` – Django secret key (change in production)
* `ENVIRONMENT` – Environment name (Development, Staging, Production, etc.)

Your `settings.py` should read these values using `os.getenv()` or `django-environ`.

Example in `settings.py`:

```python
import os
SECRET_KEY = os.getenv('SECRET_KEY', 'fallback-secret-key')
ENVIRONMENT = os.getenv('ENVIRONMENT', 'Development')
```

---

## 🚀 Getting Started

### 1️⃣ Clone the repo

```bash
git clone https://github.com/M-Arsalan09/django-rest-api-jwt-authentication.git
cd django-rest-jwt-auth
```

### 2️⃣ Create `.env`

```bash
cp .env.example .env
```

Edit `.env` to set your secret key and environment.

### 3️⃣ Run with Docker (recommended)

```bash
docker-compose up --build
```

The API will be available at `http://localhost:8000/api/`

### 4️⃣ Run without Docker

Make sure Python 3.11 is installed, then:

```bash
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt

python manage.py migrate
python manage.py runserver
```

---

## 🧪 API Endpoints

| Method | URL                     | Description                      |
| ------ | ----------------------- | -------------------------------- |
| POST   | `/api/register/`        | Register a new user              |
| POST   | `/api/token/`           | Obtain access & refresh tokens   |
| POST   | `/api/token/refresh/`   | Refresh access token             |
| POST   | `/api/token/blacklist/` | Logout (blacklist refresh token) |

### Example: Register a user

```bash
curl -X POST http://localhost:8000/api/register/ \
  -H 'Content-Type: application/json' \
  -d '{"username":"testuser","email":"test@example.com","password":"pass123"}'
```

Response:

```json
{
  "message": "User registered successfully",
  "user": {
    "id": 1,
    "username": "testuser",
    "email": "test@example.com"
  }
}
```

### Example: Login

```bash
curl -X POST http://localhost:8000/api/token/ \
  -H 'Content-Type: application/json' \
  -d '{"username":"testuser","password":"pass123"}'
```

---

## 🐳 Docker Notes

* The project uses SQLite; no extra DB container is needed.
* The SQLite file will be stored inside the container by default.
* To persist your DB on the host machine, map a volume in `docker-compose.yml`:

  ```yaml
  volumes:
    - ./data/db.sqlite3:/app/db.sqlite3
  ```

---

## 📝 License

This project is open-source under the [MIT License](LICENSE).

---

## 🙌 Contributing

Feel free to fork, open issues, and submit pull requests. Suggestions and improvements are welcome!
