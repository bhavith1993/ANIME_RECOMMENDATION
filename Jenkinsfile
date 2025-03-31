pipeline {
    agent any 

    environment {
        VENV_DIR = "venv"
        GCP_PROJECT = 'affable-operand-454200-b7'
        GCLOUD_PATH = "/var/jenkins_home/google-cloud-sdk/bin"
        KUBECTL_AUTH_PLUGIN = "/usr/lib/google-cloud-sdk/bin"
    }

    stages {

        stage("Cloning from GitHub") {
            steps {
                script {
                    echo 'Cloning from GitHub...'
                    checkout scmGit(
                        branches: [[name: '*/main']],
                        extensions: [],
                        userRemoteConfigs: [[
                            credentialsId: 'github-token',
                            url: 'https://github.com/bhavith1993/ANIME_RECOMMENDATION.git'
                        ]]
                    )
                }
            }
        }

        stage("Creating Python Virtual Environment") {
            steps {
                script {
                    echo 'Creating virtual environment and installing dependencies...'
                    sh '''
                    python3 -m venv ${VENV_DIR}
                    . ${VENV_DIR}/bin/activate
                    pip install --upgrade pip
                    pip install -e .
                    pip install dvc[gdrive]  # use [gcs] if you're pulling from GCS
                    '''
                }
            }
        }

        stage("DVC Pull from GCS") {
            steps {
                withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    script {
                        echo 'Running dvc pull...'
                        sh '''
                        . ${VENV_DIR}/bin/activate
                        dvc pull
                        '''
                    }
                }
            }
        }
        stage("Build and push image to GCR") {
            steps {
                withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    script {
                        echo 'Build and push image to GCR.......'
                        sh '''
                        export PATH="${PATH}:${GCLOUD_PATH}"
                        gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}
                        gcloud config set project ${GCP_PROJECT}
                        gcloud auth configure-docker --quiet
                        docker build -t gcr.io/${GCP_PROJECT}/anime-recommendation:latest .
                        docker push gcr.io/${GCP_PROJECT}/anime-recommendation:latest
                        '''
                    }
                }
            }
        }

        stage("Deploying to Kubernetes") {
            steps {
                withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    script {
                        echo 'Deploying to Kubernetes.......'
                        sh '''
                        export PATH="${PATH}:${GCLOUD_PATH}:${KUBECTL_AUTH_PLUGIN}"
                        gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}
                        gcloud config set project ${GCP_PROJECT}
                        gcloud container clusters get-credentials anime-cluster --region us-central1
                        kubectl apply -f deployment.yaml
                        '''
                    }
                }
            }
        }



    }
}
