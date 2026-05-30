pipeline{
    agent any 

    environment{
        VENV_DIR = 'venv1'
    }

    stages{
        stage('Cloning GitHub repo to Jenkins'){
            steps{
                script{
                   echo 'Cloning GitHub repo to Jenkins..........'
                   checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-token', url: 'https://github.com/chaitanya-coder65/HOTEL-RESERVATION-PREDICTION.git']])
                }
            }
        }
        
        stage('Setting our virtual environment and Installing Dependencies'){
            steps{
                script{
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


    }
}