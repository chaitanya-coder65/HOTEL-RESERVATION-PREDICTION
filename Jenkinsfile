pipeline {
    agent any

    environment {
        VENV_DIR = 'venv1'
        GCP_PROJECT = 'project-1448acab-8640-43bd-882'
        GCLOUD_PATH = '/var/jenkins_home/google-cloud-sdk/bin'
    }

    stages {

        stage('Cloning GitHub repo to Jenkins') {
            steps {
                script {
                    echo 'Cloning GitHub repo to Jenkins..........'

                    checkout scmGit(
                        branches: [[name: '*/main']],
                        extensions: [],
                        userRemoteConfigs: [[
                            credentialsId: 'github-token',
                            url: 'https://github.com/chaitanya-coder65/HOTEL-RESERVATION-PREDICTION.git'
                        ]]
                    )
                }
            }
        }

        stage('Setting our virtual environment and Installing Dependencies') {
            steps {
                script {
                    echo 'Setting our virtual environment and Installing Dependencies'

                    sh '''
                    python -m venv ${VENV_DIR}
                    . ${VENV_DIR}/bin/activate

                    pip install --upgrade pip
                    pip install -e .
                    '''
                }
            }
        }

        stage('Building and pushing Docker Images to GCR') {
            steps {

                withCredentials([
                    file(
                        credentialsId: 'gcp-key',
                        variable: 'GOOGLE_APPLICATION_CREDENTIALS'
                    )
                ]) {

                    script {

                        echo 'Building and pushing Docker Images to GCR'

                        sh '''
                        export PATH=$PATH:${GCLOUD_PATH}

                        gcloud auth activate-service-account \
                        --key-file=${GOOGLE_APPLICATION_CREDENTIALS}

                        gcloud config set project ${GCP_PROJECT}

                        gcloud auth configure-docker --quiet

                        docker build -t gcr.io/${GCP_PROJECT}/ml-project:latest .

                        docker push gcr.io/${GCP_PROJECT}/ml-project:latest
                        '''
                    }
                }
            }
        }
    }
}