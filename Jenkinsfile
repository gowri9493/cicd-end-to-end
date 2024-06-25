pipeline {
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}" 
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') 
            }

            stages {
                stage ('checkout') {
                    steps {
                        git credentialsId: "githubcredentials",
                        url: "https://github.com/gowri9493/cicd-end-to-end.git",
                        branch: "main"
                    }
                }

                stage ('BUILD_IMAGE') {
                    steps {
                        echo "build docker image"
                        sh 'docker build -t gowri9493/cicd-e2e:${IMAGE_TAG} .'
                    }

                }

                stage ('push artifacts to registry') {
                    steps {
                        script {
                            docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                                echo 'push to repo' 
                                sh 'docker push gowri9493/cicd-e2e:${IMAGE_TAG}'
                            }
                        }
                    }
                }

                stage ('checkout K8S manifest SCM') {
                steps {
                    git credentialsId: "githubcredentials",
                    url: "https://github.com/gowri9493/cicd-demo-manifests-repo.git",
                    branch: "main"
                }
                }

                stage ('update K8S manifest and push to repo') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'githubcredentials', passwordVariable: 'GIT_TOKEN',
                            usernameVariable: 'GIT_USERNAME')]) {
                        
                            }
                                sh '''
                                cat deploy.yaml
                                ls -l deploy.yaml
                                sed -i "s/gowri9493\\/cicd-e2e:v1/gowri9493\\/cicd-e2e:${IMAGE_TAG}/g" deploy.yaml
                                cat deploy.yaml

                                git config --global user.email "gowrishankarallada@gmail.com"
                                git config --global user.name "gowri"
                                git add deploy.yaml
                                git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                                git remote -v
                                git push https://${GIT_USERNAME}:${GIT_TOKEN}@github.com/gowri9493/cicd-demo-manifests-repo.git HEAD:main
                                 ''' 

                            }
                        }
                    }
                }

            }

