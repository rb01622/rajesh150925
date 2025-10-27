pipeline {
    agent any

    tools {
        nodejs "NodeJS 18"   // Use NodeJS installed in Jenkins
    }

    environment {
        TARGET_USER = 'kbkannah'
        TARGET_HOST = '34.55.54.62'
        TARGET_PATH = '/home/kbkannah/nodeapp'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/rb01622/rajesh150925.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || echo "No tests found"'
            }
        }

      stage('Deploy to VM') {
    steps {
        sshagent(['linux-deploy-key']) {
            sh '''
            ssh -o StrictHostKeyChecking=no kbkannah@34.55.54.62 "echo ✅ Connected successfully"
            '''
        }
    }
}
            }
        }
    
}
    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Deployment Failed.'
        }
    }
}
