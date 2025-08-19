# ZTN-IAM

ZTN-IAM (Zero Trust Network - Identity and Access Management) is a Flask based service for managing digital identities, enforcing security policies and auditing user activity.
It implements a Zero Trust model in mobile first environment where every request is authenticated and authorised regardless of network location.

## Features

- **User management** – registration, password hashing and sessionless JWT authentication.
- **Multi‑factor authentication** – TOTP, SMS/Email OTP and WebAuthn (FIDO2) support.
- **Role‑based access control** – assign roles and permissions using `UserRole` and `UserAccessControl` models.
- **Wallet & transaction module** – manage user balances and track mobile‑money style transactions.
- **SIM card registry** – bind SIM cards to users and support secure SIM swap workflow.
- **Real‑time and audit logging** – capture authentication attempts, high‑risk events and geolocation data.
- **REST and web routes** – extensible blueprints for auth, user, wallet, transaction, admin, agent and IAM APIs.
- **Docker friendly** – deploys with PostgreSQL database and an optional NGINX reverse proxy.

## Project layout
```
├── config.py             # Configuration and environment variables
├── main.py               # Flask application factory and blueprint registration
├── models/               # SQLAlchemy models
├── routes/               # API and web route blueprints
├── templates/            # Jinja2 templates for web views
├── static/               # Static assets
├── docker-compose.yml    # Containerised development stack
└── Dockerfile            # Image used by the app service
```

## Getting started
### Prerequisites
- Docker and docker‑compose (recommended) or Python 3.10+
- A PostgreSQL instance if running without docker
- Google mail account for outbound email alerts (Flask‑Mail)

### Configuration
Create a `.env` file in the project root with at least the following variables:
```dotenv
JWT_SECRET_KEY=change-me
FLASK_SECRET_KEY=change-me
MAIL_USERNAME=your@gmail.com
MAIL_PASSWORD=app-password
USE_NGINX_PROXY=True
SSL_CERT_PATH=/etc/ssl/certs/your-cert.pem
SSL_KEY_PATH=/etc/ssl/private/your-key.pem
# Optional: override DB URI
#SQLALCHEMY_DATABASE_URI=postgresql://user:pass@host:5432/ztn_db
```

### Running with Docker
```bash
docker-compose up --build
```
This launches PostgreSQL, the Flask application (served by Gunicorn) and NGINX terminating TLS on port **443**.

### Running locally
```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
flask db upgrade  # apply migrations
python main.py
```
The service will be available on <http://127.0.0.1:5000> unless the `USE_NGINX_PROXY` flag is enabled.

### Tests
Run the test suite (if tests are present):
```bash
pytest
```

## License
This project is released under the [MIT License](LICENSE).

## Roadmap
- Multi‑tenant administration interface
- Additional authentication providers
- Enhanced policy engine for fine‑grained authorisation
