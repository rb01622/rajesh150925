pipeline {
    agent any

    tools {
        nodejs "NodeJS 18"
    }

    environment {
        DEPLOY_SERVER = "34.55.54.62"       // target VM IP
        DEPLOY_USER = "kbkannah"            // target VM username
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rb01622/rajesh150925.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to Target VM') {
            steps {
                sshagent(['linux-deploy-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ${DEPLOY_USER}@${DEPLOY_SERVER} '
                        cd /var/www/nodeapp || mkdir -p /var/www/nodeapp && cd /var/www/nodeapp
                        git pull origin main || git clone https://github.com/rb01622/rajesh150925.git .
                        npm install
                        npm run start
                    '
                    '''
                }
            }
        }
    }
}
