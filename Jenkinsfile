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
        // stage('Start test app') {
        //     steps {
        //         sh '''
        //         docker-compose up -d 
        //         '''

        //     }
        //     post {
        //         success {
        //             echo 'App started successfully :)'
        //         }
        //         failure {
        //             echo 'App failed :('
        //         }
        //     }
        // }
        // stage( 'Stop test app') {
        //     steps {
        //         sh 'docker-compose down'
        //     }
        // }
        stage ('Push image to Docker Hub') {
            steps {
                echo "Workspace is $WORKSPACE"
                dir("$WORKSPACE/azure-vote"){
                    script {
                        docker.withRegistry('https://index.docker.io/v2/', 'DockerHub')
                        def image = docker.build('sourabhhbar/jenkins-azure-demo-app:latest')
                        image.push()
                    }
                }
            }
            post {
                success {
                    echo 'Image pushed successfully :)'
                }
                failure {
                    echo 'Image pushed failed :('
                }
            }
        }
    }
}
