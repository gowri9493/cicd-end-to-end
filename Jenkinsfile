pipeline {
    agent any 
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') 
        IMAGE_TAG = "${BUILD_NUMBER}"
        
    }
    
    stages {
        stage ('code checkout') {
            steps {
              git credentialsId: "githubcred",
              url: "https://github.com/gowri9493/cicd-end-to-end.git",
              branch: "main"
               
            }
        }
        stage ('build image') {
            steps {
                echo 'Build docker Image'
                sh 'docker build -t gowri9493/cicd-e2e:${BUILD_NUMBER} .'
                }
            }
            stage ('push artifacts to dockerhub registry') {
                steps {
                    script {
                        docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {

                        }
                    }
                    echo 'push to registry'
                    set -x
                    sh 'docker push gowri9493/cicd-e2e:${IMAGE_TAG}'
                    set +x
                }
            }

        }

    }


