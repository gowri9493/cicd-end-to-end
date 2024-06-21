pipeline {
    agent any 

    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }

    stages {
        stage ('checkout') {
            steps {
                git credentialsId: '4a313cc0-0775-4139-83df-05384f27d375',
                url: 'https://github.com/gowri9493/cicd-end-to-end.git',
                branch: 'main'
            }
        }
    
    stage ('Build Image') {
        steps {
        echo 'Build docker Image'
        sh 'docker build -t gowri9493/cicd-e2e:${BUILD_NUMBER} .'
        }
    }
    stage ('Push artifacts to registry') {
        steps {
            script {
                docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
        echo 'Push to repo'
        sh 'docker push gowri9493/cicd-e2e:${IMAGE_TAG}'
                }
       }
        }
        }
    
    stage ('Checkout K8S manifest SCM') {
        steps {
            git credentialsId: '4a313cc0-0775-4139-83df-05384f27d375',
            url: 'https://github.com/gowri9493/cicd-demo-manifests-repo.git',
            branch: 'main'
        }
    }
        stage ('update K8S manifest and push to repo') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: '4a313cc0-0775-4139-83df-05384f27d375', passwordVariable: 'GIT_PASSWORD',
                    usernameVariable: 'GIT_USERNAME' )]) {
                    sh '''
                        cat deploy.yaml
                        sed -i '' 's/32/${IMAGE_TAG}/g' deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/gowri9493/cicd-demo-manifests-repo.git HEAD:main
                        '''                        
}
                }
            }
        }
    }
}
