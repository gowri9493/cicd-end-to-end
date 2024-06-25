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

            }
}
