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
                '''
                script {
                    def image = docker.build("sourabhhbar/jenkins-azure-demo-app:latest", "./azure-vote")
                }
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
                    script {
                        docker.withRegistry('https://index.docker.io/v1/', 'DockerHub') {
                        def image = docker.build("sourabhhbar/jenkins-azure-demo-app:latest", "./azure-vote")
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

        stage ("Container Scanning") {
            parallel {
                stage ("Run Anchor") {
                    steps {
                        writeFile file: 'anchore_images', text: 'sourabhhbar/jenkins-azure-demo-app'
                        anchore name: 'anchore_images', bailOnFail: false, bailOnPluginFail: false 
                    }

                }
                stage ('Run Trivy Scan') {
                    steps {
                        sh '''
                        trivy image sourabhhbar/jenkins-azure-demo-app
                        '''
                    }
                }
            }
        }
    }
}
