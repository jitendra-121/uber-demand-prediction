# Uber Demand Prediction

A comprehensive machine learning project for predicting cab demand across New York City using time series analysis, geospatial clustering, and automated MLOps practices.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Installation](#installation)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Data Pipeline](#data-pipeline)
- [Model Training](#model-training)
- [Deployment](#deployment)
- [API Documentation](#api-documentation)
- [Testing](#testing)
- [Contributing](#contributing)
- [License](#license)

## Overview

This project implements an end-to-end machine learning solution to predict Uber demand patterns in New York City. The system combines historical trip data with temporal and geospatial features to forecast pickup demand across different regions and time intervals.

**Key Objectives:**
- Predict demand for ride-sharing services in NYC
- Optimize resource allocation for drivers and vehicles
- Provide real-time insights through an interactive web application
- Implement MLOps best practices with automated CI/CD pipelines

## Features

- **Time Series Forecasting**: Advanced temporal modeling with EWMA and feature engineering
- **Geospatial Analysis**: K-means clustering for region-based demand prediction
- **Real-time Predictions**: Interactive Streamlit application with live forecasting
- **MLOps Pipeline**: Automated model training, testing, and deployment
- **Model Registry**: MLflow integration with DagHub for model versioning
- **Data Versioning**: DVC for reproducible data pipeline management
- **CI/CD Integration**: GitHub Actions for automated testing and deployment
- **Containerization**: Docker support for scalable deployment
- **Cloud Deployment**: AWS integration for production hosting

## Architecture

```
Data Sources -> Data Processing -> Feature Engineering -> Model Training -> Model Registry -> Deployment
     |              |                    |                    |               |              |
   Raw NYC        DVC Pipeline      Geospatial +         Linear Reg +     MLflow +      Streamlit +
   Taxi Data      Management        Temporal Feat        Scikit-learn      DagHub         AWS EC2
```

## Installation

### Prerequisites

- Python 3.12.7 or higher
- Git
- AWS CLI (for deployment)
- Docker (optional)

### Setup

1. **Clone the repository:**
```bash
git clone https://github.com/yourusername/uber-demand-prediction.git
cd uber-demand-prediction
```

2. **Create virtual environment:**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies:**
```bash
pip install -r requirements.txt
```

4. **Install development dependencies:**
```bash
pip install -r requirements-dev.txt
```

5. **Install the project in editable mode:**
```bash
pip install -e .
```

## Usage

### Data Pipeline

1. **Initialize DVC and pull data:**
```bash
dvc init
dvc pull
```

2. **Run the complete pipeline:**
```bash
dvc repro
```

3. **Run individual pipeline stages:**
```bash
# Data ingestion
python src/data/data_ingestion.py

# Feature extraction
python src/features/extract_features.py

# Feature processing
python src/features/feature_processing.py

# Model training
python src/models/train.py
```

### Model Training

```bash
# Train the model
make train

# Evaluate model performance
python src/models/evaluate.py

# Register model to MLflow
python src/models/register_model.py
```

### Web Application

```bash
# Run Streamlit app locally
streamlit run app.py
```

### Docker Deployment

```bash
# Build Docker image
docker build -t uber-demand-prediction .

# Run container
docker run -p 8501:8501 uber-demand-prediction
```

## Project Structure

```
├── README.md                    <- Project documentation
├── requirements.txt             <- Production dependencies
├── requirements-dev.txt         <- Development dependencies
├── requirements-docker.txt      <- Docker-specific dependencies
├── setup.py                     <- Package installation configuration
├── Makefile                     <- Automation commands
├── Dockerfile                   <- Container configuration
├── app.py                       <- Streamlit web application
├── dvc.yaml                     <- DVC pipeline configuration
├── dvc.lock                     <- DVC pipeline lock file
├── params.yaml                  <- Model parameters
├── promote_model.py             <- Model promotion script
├── test_environment.py          <- Environment validation
├── tox.ini                      <- Testing configuration
├── appspec.yml                  <- AWS CodeDeploy configuration
├──.github/
│   └── workflows/
│       └── ci_cd.yaml          <- GitHub Actions CI/CD pipeline
├── data/
│   ├── external/               <- External data sources
│   ├── interim/                <- Intermediate processed data
│   └── processed/              <- Final modeling datasets
├── deploy/
│   └── scripts/                <- Deployment automation scripts
├── docs/                       <- Sphinx documentation
├── models/                     <- Trained models and artifacts
├── notebooks/                  <- Jupyter notebooks for analysis
│   ├── EDA-Demand-Prediction.ipynb
│   ├── Model-Selection.ipynb
│   ├── Breaking_NYC_to_Regions.ipynb
│   └── Training-Baseline-Model.ipynb
├── references/                 <- Data dictionaries and manuals
├── reports/
│   └── figures/               <- Generated visualizations
├── src/                       <- Source code modules
│   ├── data/                  <- Data processing scripts
│   │   └── data_ingestion.py
│   ├── features/              <- Feature engineering
│   │   ├── extract_features.py
│   │   └── feature_processing.py
│   ├── models/                <- Model training and evaluation
│   │   ├── train.py
│   │   ├── evaluate.py
│   │   └── register_model.py
│   └── visualization/         <- Plotting and visualization
└── tests/                     <- Unit and integration tests
    ├── test_model_performance.py
    └── test_model_registry.py
```

## Data Pipeline

The project uses DVC for data pipeline management with the following stages:

1. **Data Ingestion**: Download and preprocess NYC taxi trip data
2. **Feature Extraction**: 
   - Geospatial clustering using Mini-Batch K-Means
   - Temporal feature engineering
   - EWMA smoothing for demand patterns
3. **Feature Processing**: Train/test split and final preprocessing
4. **Model Training**: Linear regression with feature transformations
5. **Model Evaluation**: Performance metrics and validation
6. **Model Registration**: MLflow model registry integration

## Model Training

The system implements a multi-step training process:

- **Geospatial Features**: K-means clustering to identify demand regions
- **Temporal Features**: Hour, day, month, and seasonal patterns
- **Demand Smoothing**: Exponentially Weighted Moving Average (EWMA)
- **Feature Scaling**: StandardScaler for numerical features
- **Encoding**: OneHotEncoder for categorical variables
- **Model**: Linear Regression with regularization

## Deployment

### Local Development
```bash
streamlit run app.py
```

### Production Deployment

1. **AWS EC2 Deployment:**
   - Automated via GitHub Actions
   - CodeDeploy integration
   - Auto-scaling group configuration

2. **Docker Deployment:**
   - Multi-stage Docker builds
   - Production-ready containers
   - Health check endpoints

3. **Model Serving:**
   - MLflow model registry integration
   - Automatic model versioning
   - A/B testing capabilities

## API Documentation

The Streamlit application provides:

- **Interactive Dashboard**: Real-time demand visualization
- **Prediction Interface**: Input location and time for demand forecasts
- **Historical Analysis**: Trend analysis and pattern recognition
- **Model Metrics**: Performance monitoring and model statistics

## Testing

```bash
# Run all tests
pytest

# Run specific test categories
pytest tests/test_model_performance.py
pytest tests/test_model_registry.py

# Run with coverage
pytest --cov=src

# Test environment setup
python test_environment.py
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow PEP 8 style guidelines
- Add tests for new features
- Update documentation for API changes
- Ensure all tests pass before submitting PR

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- NYC Taxi and Limousine Commission for providing the dataset
- DagHub for MLflow hosting and model registry
- Campus X for project guidance and support
- The open-source community for the amazing tools and libraries

## Contact

For questions or support, please open an issue on GitHub or contact the development team.

---

**Built with**: Python, Scikit-learn, MLflow, DVC, Streamlit, Docker, AWS, GitHub Actions
