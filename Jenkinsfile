pipeline {
    agent any

    stages {
        stage('Verify branch') {
            steps {
                echo "$GIT_BRANCH"
            }
        }
        stage('Docker Image build') {
            steps {
                sh '''
                cd azure-vote
                docker images -a
                docker build -t jenkins-azure-demo-app .
                docker images -a
                '''
            }
        }    
        stage('Start test app') {
            steps {
                sh '''
                docker-compose up -d 
                '''

            }
            post {
                success {
                    echo 'App started successfully :)'
                }
                failure {
                    echo 'App failed :('
                }
            }
        }
        stage( 'Stop test app') {
            steps {
                sh 'docker-compose down'
            }
        }
    }
}
