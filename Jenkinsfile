pipeline {
    agent any

    tools {
        nodejs "NodeJS 18"
    }

    environment {
        DEPLOY_SERVER = "34.55.54.62"       // Target VM IP
        DEPLOY_USER = "kbkannah"            // Target VM username
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

        // ❌ Removed Build stage — not needed for this Node.js app

        stage('Deploy to Target VM') {
            steps {
                sshagent(['linux-deploy-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ${DEPLOY_USER}@${DEPLOY_SERVER} '
                        cd /var/www/nodeapp || mkdir -p /var/www/nodeapp && cd /var/www/nodeapp
                        if [ ! -d .git ]; then
                            git clone https://github.com/rb01622/rajesh150925.git .
                        else
                            git pull origin main
                        fi
                        npm install
                        nohup node server.js > app.log 2>&1 &
                    '
                    '''
                }
            }
        }
    }
}
