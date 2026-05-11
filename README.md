# Minimal Django Automated Deployment Pipeline

This project demonstrates a complete automated deployment pipeline for a minimal Django application.

## Features
- **Health Endpoint**: `/health/` returns `{"status": "ok"}`.
- **Dockerized**: App and PostgreSQL setup using Docker Compose.
- **CI/CD**: GitHub Actions for linting, testing, and building Docker images.

## Local Setup

### 1. Prerequisite
- Python 3.11+
- Docker & Docker Compose

### 2. Manual Run (Without Docker)
1. Create a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Run migrations (SQLite by default):
   ```bash
   python manage.py migrate
   ```
4. Start the server:
   ```bash
   python manage.py runserver
   ```
5. Visit `http://127.0.0.1:8000/health/`.

### 3. Docker Run
1. Build and start services:
   ```bash
   docker-compose up --build
   ```
2. The app will be available at `http://localhost:8000/health/`.

## CI/CD Pipeline Explanation

The GitHub Actions workflow (`.github/workflows/ci.yml`) consists of three stages:

1. **Linting (`flake8`)**: 
   - **Why**: Ensures code quality and adherence to PEP 8 standards.
   - **Order**: First, because there's no point running tests if the code style is broken.

2. **Testing (`Django test runner`)**:
   - **Why**: Verifies that the application logic works as expected.
   - **Order**: Second, ensuring that only valid code that passes style checks is tested.

3. **Docker Build**:
   - **Why**: Verifies that the application can be successfully containerized.
   - **Order**: Final stage, serving as a smoke test for the deployment artifact.

## Production-Grade Recommendations

To make this application production-ready, I would add:

1. **Database Management**: Use a managed database service (e.g., AWS RDS, GCP Cloud SQL) instead of a containerized Postgres for better reliability and backups.
2. **Security**:
   - Use `django-environ` for stricter environment variable management.
   - Set `DEBUG=False` and configure `SECURE_PROXY_SSL_HEADER`.
   - Implement rate limiting (e.g., Django Ratelimit).
3. **Monitoring & Logging**:
   - Integrate Sentry for error tracking.
   - Use Prometheus/Grafana or Datadog for performance monitoring.
4. **Static Files**: Use WhiteNoise or AWS S3 to serve static files efficiently.
5. **Infrastructure as Code (IaC)**: Use Terraform or Pulumi to manage cloud resources.
6. **Orchestration**: Deploy to Kubernetes (EKS/GKE) or a PaaS like Render/Railway with auto-scaling configured.
