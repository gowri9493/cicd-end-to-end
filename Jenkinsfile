pipeline {
    agent any 

    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }

    stages {
        stage('Checkout Source') {
            steps {
                git credentialsId: 'e64e7109-530d-445d-a1d1-b2f64889e323',
                    url: 'https://github.com/gowri9493/cicd-end-to-end.git',
                    branch: 'main'
            }
        }

        stage('Build Image') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t gowri9493/cicd-e2e:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Registry') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        echo 'Pushing Docker image to registry'
                        sh 'docker push gowri9493/cicd-e2e:${IMAGE_TAG}'
                    }
                }
            }
        }

        stage('Checkout K8S Manifest SCM') {
            steps {
                git credentialsId: 'e64e7109-530d-445d-a1d1-b2f64889e323',
                    url: 'https://github.com/gowri9493/cicd-demo-manifests-repo.git',
                    branch: 'main'
            }
        }

        stage('Update K8S Manifest and Push to Repo') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'e64e7109-530d-445d-a1d1-b2f64889e323', passwordVariable: 'GIT_TOKEN', usernameVariable: 'GIT_USERNAME')]) {
                        sh '''
                            echo "Updating deploy.yaml"
                            cat deploy.yaml
                            sed -i "s/gowri9493\\/cicd-e2e:v1/gowri9493\\/cicd-e2e:${IMAGE_TAG}/g" deploy.yaml
                            echo "Updated deploy.yaml"
                            cat deploy.yaml

                            git config --global user.email "gowrishankarallada@gmail.com"
                            git config --global user.name "gowri"
                            git add deploy.yaml
                            git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                            git push https://${GIT_USERNAME}:${GIT_TOKEN}@github.com/gowri9493/cicd-demo-manifests-repo.git HEAD:main
                        '''
                    }
                }
            }
        }
    }
}
