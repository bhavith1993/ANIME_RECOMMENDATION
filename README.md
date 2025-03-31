Anime Recommendation System

🌐 Overview

The Anime Recommendation System is a full-stack ML-powered application that delivers personalized anime suggestions using collaborative filtering and content-based filtering techniques. Designed with a focus on performance, automation, and deployment scalability, this project showcases the integration of machine learning, MLOps, and DevOps practices.

This project is ideal for demonstrating ML model development, CI/CD automation with Jenkins, data versioning using DVC, and seamless deployment on Google Cloud Platform (GCP).

🚀 Key Features

✨ Personalized Recommendations based on user preferences and similarity metrics

📈 Model versioning and reproducibility using DVC

⚖️ Experiment tracking and logging with CometML

⛏️ Data Preprocessing & Feature Engineering with pandas, scikit-learn, numpy

🚪 Web Application built with Flask

⚙️ CI/CD Pipeline using Jenkins

🌍 Deployment using Docker, Google Container Registry (GCR), and Kubernetes Engine (GKE)

📚 Tech Stack

🤖 Machine Learning

Python 3.11

scikit-learn

pandas

numpy

joblib

TensorFlow (for future deep-learning-based enhancements)

📂 Data Version Control

DVC (with Google Cloud Storage remote)

Google Cloud Storage (GCS)

🚌 MLOps & DevOps

Jenkins (CI/CD automation)

Docker (Containerization)

Google Container Registry (GCR)

Google Kubernetes Engine (GKE)

GitHub (Version control)

📝 Experimentation & Logging

CometML (for hyperparameter tuning & metrics tracking)

Logging via Python's logging module

🚪 Frontend (Web Interface)

Flask

HTML5 / CSS3 / Bootstrap

🔧 Setup & Installation

1. Clone the Repository

git clone https://github.com/bhavith1993/ANIME_RECOMMENDATION.git
cd ANIME_RECOMMENDATION

2. Create a Virtual Environment

python -m venv venv
source venv/bin/activate   # On Windows: venv\Scripts\activate

3. Install Requirements

pip install --upgrade pip
pip install -e .
pip install dvc[gcs]  # If using GCS for remote

4. DVC Pull for Data & Models

Ensure you have access to the GCS service account key and run:

export GOOGLE_APPLICATION_CREDENTIALS="/path/to/your/gcp-key.json"
dvc pull

5. Launch the Flask App

python application.py

Then navigate to http://localhost:5000 to use the application.

📚 Jenkins CI/CD Pipeline

Jenkinsfile Highlights:

Stage 1: Clone GitHub repo

Stage 2: Setup Python virtual environment & install dependencies

Stage 3: Run DVC pull using GCP credentials

Stage 4: Build Docker image & push to GCR

Stage 5: Deploy to GKE using kubectl

The Jenkinsfile is included in the root directory and uses secure credentials managed by Jenkins:

github-token for cloning the repo

gcp-key for authenticating with GCP services

🌐 Cloud Deployment (GCP)

Infrastructure Setup:

GCS bucket for storing DVC-tracked datasets and model artifacts

GCR for container registry

GKE cluster for deploying the application

Deployment Workflow:

Jenkins pipeline builds Docker image from the repo

Authenticates and pushes to gcr.io/<project-id>/anime-recommendation

Deploys to GKE using kubectl apply -f deployment.yaml

🔗 Directory Structure

ANIME_RECOMMENDATION/
|-- application.py
|-- Jenkinsfile
|-- deployment.yaml
|-- requirements.txt
|-- setup.py
|-- venv/
|-- .dvc/
|-- logs/
|-- src/
|   |-- recommender.py
|-- notebook/
|   |-- anime_model_dev.ipynb
|-- artifacts/

🙏 Acknowledgments

DVC for reproducible ML pipelines

CometML for tracking experiments

Google Cloud for infrastructure services

Jenkins for automated CI/CD