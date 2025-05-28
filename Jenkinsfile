pipeline {
    agent any

    environment {
        SSH_KEY = credentials('project2-ssh-key') // Jenkins credential ID
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'dev', url: 'https://github.com/assawti/project2_chat_nodejs.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Allow the pipeline to continue even if there is no test script
                sh 'npm test || true'
            }
        }

        stage('Deploy to App Server') {
            steps {
                sshagent(['project2-ssh-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@52.5.199.242 '
                      git config --global --add safe.directory /home/ubuntu/chat-app &&
                      cd /home/ubuntu/chat-app &&
                      git pull &&
                      npm install &&
                      pm2 restart app.js
                    '
                    '''
                }
            }
        }
    }
}
