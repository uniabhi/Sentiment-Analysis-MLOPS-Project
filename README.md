# Sentiment Analysis MLOps Project

An end-to-end MLOps project for sentiment analysis on the IMDB dataset. This project demonstrates a complete machine learning lifecycle, including data versioning, experiment tracking, containerization, CI/CD automated deployment to AWS EKS, and real-time monitoring.

## 🚀 Project Overview

This project builds a sentiment analysis model using a modular pipeline managed by DVC. It leverages modern MLOps tools to ensure reproducibility, scalability, and observability.

* **Data Versioning**: DVC with AWS S3 backend.
* **Experiment Tracking**: MLflow integrated with DagsHub.
* **Containerization**: Docker.
* **Orchestration**: Kubernetes (AWS EKS).
* **CI/CD**: GitHub Actions.
* **Monitoring**: Prometheus & Grafana.

## 🛠️ Tech Stack

* **Language**: Python 3.10
* **Frameworks**: Flask (Web App), Scikit-learn (Model)
* **MLOps**: DVC, MLflow, DagsHub
* **DevOps**: Docker, AWS ECR, AWS EKS, GitHub Actions
* **Monitoring**: Prometheus, Grafana
* **Infrastructure**: AWS (S3, EC2, EKS, IAM)

## 📂 Project Structure

```text
├── .github/workflows/   # CI/CD pipeline (ci.yaml)
├── data/                # Data managed by DVC (raw, interim, processed)
├── docs/                # Documentation
├── flask_app/           # Flask application for model serving
├── models/              # Serialized models (model.pkl, vectorizer.pkl)
├── notebooks/           # Jupyter notebooks for experiments
├── reports/             # Metrics and experiment logs
├── scripts/             # Utility scripts (e.g., promote_model.py)
├── src/                 # Source code for the ML pipeline
│   ├── data/            # Ingestion and preprocessing
│   ├── features/        # Feature engineering
│   ├── model/           # Model building and evaluation
│   └── logger/          # Logging utilities
├── tests/               # Unit tests
├── dvc.yaml             # DVC pipeline definition
├── params.yaml          # Hyperparameters
└── requirements.txt     # Python dependencies

```

## ⚙️ Setup & Installation

### 1. Prerequisites

* Anaconda or Miniconda
* Git
* AWS CLI (configured with credentials)
* Docker Desktop
* `kubectl` and `eksctl`

### 2. Environment Setup

Create and activate the virtual environment:

```bash
conda create -n atlas python=3.10
conda activate atlas
pip install -r requirements.txt

```

### 3. DVC Setup (Data Version Control)

Initialize DVC and configure the remote storage (S3):

```bash
dvc init
# Add S3 remote (Update <bucket-name> with your specific bucket)
dvc remote add -d myremote s3://sentiment-mlopsss

```

*Note: Ensure you have AWS credentials configured to access the S3 bucket.*

## 🏃‍♂️ Running the ML Pipeline

The machine learning pipeline is defined in `dvc.yaml` and consists of the following stages:

1. **Data Ingestion**: Downloads data.
2. **Data Preprocessing**: Cleans and prepares text data.
3. **Feature Engineering**: Vectorizes text (Bag of Words/TF-IDF).
4. **Model Building**: Trains the model.
5. **Model Evaluation**: Generates metrics and reports.
6. **Model Registration**: Registers the model for production.

To run the entire pipeline:

```bash
dvc repro

```

To visualize the pipeline status:

```bash
dvc status

```

## 📊 Experiment Tracking

This project uses **MLflow** with **DagsHub** for tracking experiments.

1. Link your repository to DagsHub.
2. Set the MLflow tracking URI in your environment or code.
3. Run experiments and view metrics (Accuracy, Precision, Recall) on the DagsHub MLflow UI.

## 🐳 Containerization (Docker)

To build and run the Flask application locally:

```bash
# Build the image
docker build -t capstone-app:latest .

# Run the container (Ensure CAPSTONE_TEST env var is set if needed)
docker run -p 8888:5000 -e CAPSTONE_TEST=<your_token> capstone-app:latest

```

Access the app at `http://localhost:8888`.

## ☁️ Deployment (AWS EKS)

The deployment is automated via GitHub Actions, but manual steps are as follows:

1. **Create EKS Cluster**:
```bash
eksctl create cluster --name flask-app-cluster --region us-east-1 --nodegroup-name flask-app-nodes --node-type t3.small --nodes 1

```


2. **Update Kubeconfig**:
```bash
aws eks --region us-east-1 update-kubeconfig --name flask-app-cluster

```


3. **Deploy Application**:
```bash
kubectl apply -f deployment.yaml

```


4. **Access Service**:
```bash
kubectl get svc flask-app-service

```


Use the `EXTERNAL-IP` to access the live application.

## 📈 Monitoring (Prometheus & Grafana)

Monitoring is set up on separate EC2 instances.

### Prometheus

* **Setup**: Installed on an Ubuntu EC2 instance.
* **Config**: Scrapes metrics from the Flask app's `/metrics` endpoint.
* **Access**: Port `9090` (e.g., `http://<ec2-ip>:9090`).

### Grafana

* **Setup**: Installed on a separate Ubuntu EC2 instance.
* **Data Source**: Connected to the Prometheus server.
* **Dashboard**: Visualizes request counts, latency, and model predictions.
* **Access**: Port `3000` (e.g., `http://<ec2-ip>:3000`).

## 🔄 CI/CD Pipeline

The `.github/workflows/ci.yaml` automates the following on every push:

1. **Testing**: Runs unit tests (`tests/test_model.py`, `tests/test_flask_app.py`).
2. **Pipeline Execution**: Runs `dvc repro` to ensure model validity.
3. **Containerization**: Builds and pushes the Docker image to AWS ECR.
4. **Deployment**: Updates the Kubernetes deployment on AWS EKS.

## 📜 License

[MIT](https://www.google.com/search?q=LICENSE)

```

***

### **Key Details Included from Your Files:**
* [cite_start]**Project Flow**: The "Setup & Installation" and "Deployment" sections closely follow the steps outlined in your `projectflow.txt` (e.g., `conda create`, `dvc remote add`, `eksctl create cluster`)[cite: 11481, 11483, 11491].
* [cite_start]**Pipeline Stages**: The "Running the ML Pipeline" section mirrors the stages found in `dvc.yaml` (ingestion, preprocessing, feature engineering, model building, etc.)[cite: 11482].
* [cite_start]**AWS & S3**: The specific S3 bucket `s3://sentiment-mlopsss` mentioned in your notes is included in the configuration steps[cite: 11483].
* [cite_start]**Monitoring**: The specific setup for Prometheus (port 9090) and Grafana (port 3000) on EC2 instances is documented[cite: 11494, 11496].
* [cite_start]**CI/CD**: The workflow described matches the `ci.yaml` file, including the use of secrets like `CAPSTONE_TEST` and deployment to ECR/EKS[cite: 11484, 11486].
