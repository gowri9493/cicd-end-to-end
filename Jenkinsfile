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
                steps {
                git credentialsId: "githubcred",
                url: "https://github.com/gowri9493/cicd-demo-manifests-repo.git",
                branch: "main"
                }
                }

                stage ('update K8s manifest and push to repo') {
                    steps {
                        script {
                            withCredentials ([UsernamePassword( credentialsId: 'githubcred', passwordVariable: 'GIT_TOKEN',
                            usernameVariable: 'GIT-USERNAME')]) {
                                sh '''
                                cat deploy.yaml
                                ls -l deploy.yaml
                                sed -i "s/gowri9493\\/cicd-e2e:v1/gowri9493\\/cicd-e2e:${"IMAGE_TAG"}/g" deploy.yaml
                                cat deploy.yaml
                                git add deploy.yaml
                                git commit -m 'Updated the deploy.yaml | jenkins Pipeline'
                                git remote -v
                                git push https://${GIT_USERNAME}:${GIT_TOKEN}@github.com/gowri9493/cicd-demo-manifests-repo.git HEAD:main
                                '''
                            }

                            }
                        }
                    }
                }


            }


        

    


