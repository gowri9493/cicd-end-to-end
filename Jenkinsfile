pipeline {
    agent any 
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('0706e217-7191-4f12-aa04-4cec85ea558f') 
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
                        docker.withRegistry('https://index.docker.io/v1/', '0706e217-7191-4f12-aa04-4cec85ea558f') {

                        }
                    }
                    echo 'push to registry'
                    
                    sh 'docker push gowri9493/cicd-e2e:${IMAGE_TAG}'
                    
                }
            }
            
            stage ('checkout K8s manifest') {
                git credentialsId: "githubcred",
                url: "https://github.com/gowri9493/cicd-demo-manifests-repo.git",
                branch: "main"
            }

        }

    }


