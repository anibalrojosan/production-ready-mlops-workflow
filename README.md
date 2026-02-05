# Production-Ready MLOps Workflow 🚀

[![CI/CD Pipeline](https://github.com/anibalrojosan/mlops-infrastructure-demo/actions/workflows/main.yml/badge.svg)](https://github.com/anibalrojosan/mlops-infrastructure-demo/actions/workflows/main.yml)
[![Python 3.12](https://img.shields.io/badge/python-3.12-blue.svg)](https://www.python.org/downloads/release/python-3120/)
[![Coverage](https://img.shields.io/badge/coverage-80%2B%25-green.svg)](#running-tests)

**An industrialized ML pipeline** that transforms a ML model into a scalable, tested, and containerized microservice.

---

## 📑 Table of Contents
- [⚡ Quick Start](#⚡-quick-start-30-seconds)
- [🎯 Project Purpose](#🎯-project-purpose)
- [📁 Project Structure](#📁-project-structure)
- [🛠️ Technical Documentation](#🛠️-technical-documentation)
    - [Setup](#setup)
    - [Running Tests](#running-tests)
    - [Training & Execution](#training-the-model)
- [🔄 CI/CD & Quality Control](#🔄-cicd--quality-control)
- [🚀 Future & Tech Stack](#🚀-future--tech-stack)

---
## ⚡ Quick Start (30 seconds)

If you have Docker installed, you can spin up the entire ecosystem with a single command:

```bash
docker compose -f config/docker-compose.yml up --build
```

*   **API:** `http://localhost:5000`
*   **UI:** `http://localhost:8501`

## 🎯 Project Purpose

This project demonstrates **production-ready MLOps practices** rather than focusing solely on achieving state-of-the-art model performance. The Wisconsin Breast Cancer dataset is used as a **proof-of-concept** to validate the MLOps infrastructure.

The goal is to showcase best practices in:

- **Reproducible ML Pipelines**: Using scikit-learn pipelines for consistent preprocessing and inference
- **API Design**: Building robust REST APIs with proper validation and error handling
- **Containerization**: Multi-service architecture with Docker Compose
- **CI/CD**: Automated quality gates, testing, and deployment pipelines
- **Code Quality**: Type checking, linting, and comprehensive testing

**The real question this project answers**:

>  *"How do I ensure my ML model works the same way in production as it does in development?"*

This project solves this through container immutability and environment parity.

## 📁 Project Structure

```text
production-ready-mlops-workflow/
├── .github/workflows/         # 🔄 CI/CD: Quality gates & automated deployment
├── config/                    # 🐳 Docker: Multi-container orchestration
├── data/                      # 📊 Dataset storage (raw & processed)
├── models/                    # 🧠 Trained model artifacts (joblib)
├── notebooks/                 # 📓 EDA and experimentation
├── reports/                   # 📈 Generated metrics and figures
├── src/                       # 🛠️ Source code
│   ├── app.py                 # 🌐 Inference API (Flask)
│   ├── schemas.py             # ✅ Data validation (Pydantic)
│   └── model/                 # 🚂 Training and inference logic
├── tests/                     # 🧪 Unit & Integration test suite
└── pyproject.toml             # 📦 Dependency management (uv)
```

## 🛠️ Technical Documentation

### Setup

#### Prerequisites
- Python 3.12+
- Docker and Docker Compose (for containerized deployment)
- `uv` (recommended) or `pip` for dependency management

### Using `uv` (Recommended)

`uv` is a fast Python package installer written in Rust, offering faster dependency resolution and installation.

1. **Clone the repository:**
   ```bash
   git clone https://github.com/anibalrojosan/production-ready-mlops-workflow
   cd production-ready-mlops-workflow
   ```

2. **Install `uv` globally (if needed):**
   ```bash
   # On macOS/Linux:
   curl -LsSf https://astral.sh/uv/install.sh | sh
   # Or via pip:
   pip install uv
   ```

3. **Install dependencies:**
   ```bash
   uv sync --all-groups
   ```

4. **Activate the virtual env (optional):**
   ```bash
   source .venv/bin/activate
   ```

   Note: you can run commands using `uv run` if you don't want to activate the virtual env.

<details>
<summary><b>Using pip (Alternative)</b></summary>

1. **Create and activate virtual environment:**
   ```bash
   python -m venv .venv
   # On Windows:
   .\.venv\Scripts\Activate.ps1
   # On Linux/macOS:
   source .venv/bin/activate
   ```

2. **Upgrade pip:**
   ```bash
   pip install --upgrade pip
   ```

3. **Install dependencies:**

   **Option A**: using the **requirements.txt** (recommended for production).

   ```bash
   pip install -r requirements.txt
   ```

   **Option B**: using the **pyproject.toml** (recommended for development).

   ```bash
   pip install .
   ```
</details>

### Running Tests

The project includes comprehensive tests with a coverage requirement of 80%+.

**Run all tests:**
```bash
uv run pytest
```

**Run with verbose output:**
```bash
uv run pytest -v
```

**Run with coverage report:**
```bash
uv run pytest --cov=src --cov-report=term-missing
```

### Training the Model

Train the ML pipeline and save the model artifact:

```bash
uv run python -m src.model.model_training
```

This will:
1. Load and preprocess the data
2. Train a Random Forest classifier within a scikit-learn pipeline
3. Evaluate the pipeline
4. Save the complete trained pipeline to `models/model.joblib`

**Note**: The model must be trained before running the API.

### Running the API

Start the Flask API locally:

```bash
uv run python -m src.app
```

The API will be accessible at `http://127.0.0.1:5000/`

### API Endpoints

The API exposes a `POST /predict` endpoint that accepts features as JSON and returns the prediction with probabilities. It also includes a `GET /` health check endpoint to verify service and model status.

For full validation details and data structures, refer to the Pydantic schemas in [`src/schemas.py`](src/schemas.py).

**Example using test scripts:**
- **Linux/macOS:** `./tests/integration/bash_test.sh`
- **Windows PowerShell:** `.\tests\integration\powershell_test.ps1`

### Streamlit UI

The Streamlit application provides an interactive web interface for making predictions:

```bash
streamlit run src/streamlit_app.py
```

Ensure the Flask API is running first. The UI will open at `http://localhost:8501`.

### Docker Deployment

The project uses Docker Compose to orchestrate both the Flask API and Streamlit UI services.

### Build and Run

```bash
docker compose -f config/docker-compose.yml up --build -d
```

This will:
- Build optimized images using multi-stage Docker builds
- Start both API and Streamlit services
- Make API available at `http://localhost:5000/`
- Make Streamlit UI available at `http://localhost:8501/`

### Stop Services

```bash
docker compose -f config/docker-compose.yml down
```

## 🔄 CI/CD & Quality Control

The project implements a continuous integration pipeline that acts as a quality filter (**Quality Gates**):

1. **Static Analysis**: `ruff` for linting and `mypy` for strict typing.
2. **Automated Testing**: `pytest` with a minimum coverage requirement of 80%.
3. **Container Security**: Multi-stage Docker builds for lightweight and secure images.
4. **Integration Tests**: Endpoint validation in isolated containers before deployment.

### Pipeline Details (GitHub Actions)

The workflow defined in `.github/workflows/main.yml` includes:

### Quality Gates Job
1. **Linting**: `ruff` for code style and quality
2. **Type Checking**: `mypy` for static type analysis
3. **Testing**: `pytest` with coverage reporting
4. **Coverage Requirement**: 80% minimum (pipeline fails if below)

### Build & Deploy Job (runs after quality gates pass)
1. **Train Model**: Train and save model artifact
2. **Build Docker Images**: Create optimized container images
3. **Push to Docker Hub**: Store images in registry
4. **Integration Testing**: Test services in isolated containers
5. **Health Checks**: Verify API and UI endpoints
6. **Cleanup**: Remove test containers

This ensures that only tested and validated code reaches production.

## 🚀 Future & Tech Stack

<details>
<summary><b>🔮 Future Improvements</b></summary>

Potential enhancements to further strengthen the MLOps workflow:
- **Model Versioning**: Implement MLFlow for experiment tracking and model registry.
- **Monitoring**: Add model performance monitoring and drift detection.
- **A/B Testing**: Framework for comparing model versions in production.
- **Feature Store**: Centralized feature management for multiple models.
- **Automated Retraining**: Scheduled retraining based on data drift or performance degradation.
</details>

<details>
<summary><b>📚 Technologies Used</b></summary>

- **ML Framework**: scikit-learn
- **API Framework**: Flask
- **UI Framework**: Streamlit
- **Validation**: Pydantic
- **Testing**: pytest, pytest-mock
- **Containerization**: Docker, Docker Compose
- **CI/CD**: GitHub Actions
- **Code Quality**: ruff, mypy
- **Dependency Management**: uv
</details>

---

**Remember**: The value of this project is in the **engineering practices**, not the model metrics. These practices ensure your ML models work reliably in production, regardless of the problem domain or dataset complexity.
