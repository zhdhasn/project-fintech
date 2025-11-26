pipeline {
    agent any

    environment {
        REMOTE_USER = "jahid"
        REMOTE_HOST = "10.88.250.100"
        REMOTE = "${REMOTE_USER}@${REMOTE_HOST}"
        REMOTE_PATH = "/home/jahid/FNT"
        CREDENTIALS_ID = "remote-key" 
        GIT_BRANCH = "ksqldb"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "refs/heads/${env.GIT_BRANCH}"]],
                    userRemoteConfigs: [[url: 'https://github.com/zhdhasn/project-fintech.git']]
                ])
            }
        }

        stage('Prepare Remote and Copy Files') {
            steps {
                sshagent(credentials: ["${env.CREDENTIALS_ID}"]) {

                    sh "ssh ${env.REMOTE} \"mkdir -p ${env.REMOTE_PATH}/${env.GIT_BRANCH}\""
  
                    sh "scp docker-compose.yml ${env.REMOTE}:${env.REMOTE_PATH}/${env.GIT_BRANCH}/"

                    sh "ssh ${env.REMOTE} \"ls -l ${env.REMOTE_PATH}/${env.GIT_BRANCH}/docker-compose.yml\""
                    
                    sh """
                    ssh ${env.REMOTE} 'cd ${REMOTE_PATH}/${GIT_BRANCH} && docker compose up -d'
                    """
                }
            }
        }
    }
}