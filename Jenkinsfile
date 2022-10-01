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
    }
}
