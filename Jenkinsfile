pipeline {
    agent any 

    environment {
        VENV_DIR = "venv"
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
    }
}
