# Lab 3: Testing & CI/CD for ML Systems

![CI](https://github.com/superda10/https-github.com-superda-ddm501-lab3-movie-rating/actions/workflows/ci.yml/badge.svg)


## Overview

Implement comprehensive testing strategies and CI/CD pipelines for the movie rating prediction system to ensure quality and automate deployment.

**Course:** DDM501 - AI in Production: From Models to Systems  
**Weight:** 15% of total grade  
**Duration:** 3 hours (in-class) + 1 week to complete  
**Prerequisites:** Lab 1 and Lab 2 completed

## Learning Objectives

- Write comprehensive unit tests for ML components
- Implement integration tests for API endpoints
- Create data validation tests
- Design model behavioral tests (invariance, directional, minimum functionality)
- Set up CI/CD pipelines with GitHub Actions
- Implement automated code quality checks

## Project Structure

```
ddm501-lab3-starter/
├── app/
│   ├── __init__.py
│   ├── main.py             # FastAPI application
│   ├── model.py            # ML model class
│   ├── schemas.py          # Pydantic schemas
│   └── config.py           # Configuration
├── tests/
│   ├── __init__.py
│   ├── conftest.py         # Shared fixtures
│   ├── unit/
│   │   ├── __init__.py
│   │   ├── test_model.py   # Model unit tests
│   │   └── test_schemas.py # Schema tests
│   ├── integration/
│   │   ├── __init__.py
│   │   └── test_api.py     # API tests
│   ├── data/
│   │   ├── __init__.py
│   │   └── test_data_quality.py  # Data tests
│   └── model/
│       ├── __init__.py
│       └── test_model_behavior.py  # Behavioral tests
├── .github/
│   └── workflows/
│       ├── ci.yml          # CI pipeline
│       └── cd.yml          # CD pipeline
├── scripts/
│   └── train_model.py      # Model training script
├── models/                 # Saved models
├── .pre-commit-config.yaml # Pre-commit hooks
├── pyproject.toml          # Project configuration
├── requirements.txt
├── requirements-dev.txt    # Development dependencies
├── Dockerfile
└── README.md
```

## Quick Start

### 1. Clone and Setup

```bash
git clone https://github.com/superda10/https-github.com-superda-ddm501-lab3-movie-rating.git
cd https-github.com-superda-ddm501-lab3-movie-rating

# Create virtual environment
python -m venv venv
# Linux/Mac
source venv/bin/activate

# Windows (PowerShell)
venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt
pip install -r requirements-dev.txt
```

### 2. Train Model (if not exists)

```bash
python scripts/train_model.py
```

### 3. Run Tests

```bash
# Run all tests
pytest tests/ -v

# Run with coverage
pytest tests/ -v --cov=app --cov-report=html

# Run specific test category
pytest tests/unit/ -v
pytest tests/integration/ -v
pytest tests/data/ -v
pytest tests/model/ -v
```

### 4. Code Quality Checks

```bash
# Install pre-commit hooks
pip install pre-commit
pre-commit install

# Run all checks manually
pre-commit run --all-files

# Individual tools
black app/ tests/
flake8 app/ tests/ --max-line-length=100 --exclude=__pycache__
mypy app/
```

### 5. Run the API

```bash
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

## Implementation Status

Completed components:

- [x] `tests/unit/test_model.py` - Unit tests for model class
- [x] `tests/unit/test_schemas.py` - Schema validation tests
- [x] `tests/integration/test_api.py` - API endpoint tests
- [x] `tests/data/test_data_quality.py` - Data quality tests
- [x] `tests/model/test_model_behavior.py` - Behavioral tests
- [x] `.github/workflows/ci.yml` - CI pipeline
- [x] `.github/workflows/cd.yml` - CD pipeline (bonus)
- [x] `.pre-commit-config.yaml` - Pre-commit hooks


## Test Strategy

Hệ thống kiểm thử được chia thành 4 loại để đảm bảo chất lượng toàn diện:

- **Unit tests:** Kiểm tra từng hàm/lớp riêng lẻ, đảm bảo logic nhỏ hoạt động đúng.
- **Integration tests:** Kiểm tra sự phối hợp giữa các thành phần, đặc biệt là API endpoint.
- **Data tests:** Đảm bảo dữ liệu đầu vào/đầu ra đúng định dạng, giá trị hợp lệ.
- **Behavioral tests:** Kiểm tra hành vi tổng thể của mô hình (tính bất biến, tối thiểu, chiều hướng).

Việc phân loại này giúp phát hiện lỗi ở nhiều cấp độ, tăng độ tin cậy và dễ bảo trì hệ thống.

## Test Types

### Unit Tests
Test individual functions and classes in isolation.

```python
def test_model_loads_successfully(model):
    assert model.is_loaded()
```

### Integration Tests
Test component interactions and API endpoints.

```python
def test_predict_valid_request(test_client):
    response = test_client.post("/predict", json={"user_id": "196", "movie_id": "242"})
    assert response.status_code == 200
```

### Data Tests
Validate data quality and schema.

```python
def test_ratings_in_valid_range(sample_ratings):
    for r in sample_ratings:
        assert 1.0 <= r["rating"] <= 5.0
```

### Behavioral Tests
Test model behavior patterns.

```python
def test_same_input_same_output(model):
    result1 = model.predict("196", "242")
    result2 = model.predict("196", "242")
    assert result1 == result2
```

## CI/CD Pipeline

### Continuous Integration
- Runs on every push and pull request
- Executes linting, type checking, and tests
- Reports code coverage

### Continuous Deployment (BONUS)
- Triggered on version tags
- Builds and pushes Docker image
- Creates GitHub Release

CD requirements:
- Configure repository secrets: `DOCKER_USERNAME`, `DOCKER_PASSWORD`
- Push a tag like `v1.0.0` to trigger CD

## Grading Rubric

| Criteria | Weight |
|----------|--------|
| Test Coverage (unit, integration, data, model) | 30% |
| CI/CD Pipeline | 30% |
| Code Quality | 20% |
| Documentation | 20% |

**Minimum Requirements:**
- 80% code coverage
- All CI checks passing
- Pre-commit hooks configured

## Resources

- [pytest Documentation](https://docs.pytest.org/)
- [GitHub Actions](https://docs.github.com/en/actions)
- [pre-commit](https://pre-commit.com/)
- [Black](https://black.readthedocs.io/)
- [Flake8](https://flake8.pycqa.org/)
- [mypy](https://mypy.readthedocs.io/)

## Submission

1. Ensure all implemented files are committed
2. Ensure all tests pass
3. Achieve minimum 80% coverage
4. Push to GitHub with CI badge
5. Submit repository link via LMS

## License

MIT License - For educational purposes only.
