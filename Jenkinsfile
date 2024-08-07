pipeline {
    agent any 

    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKERHUB_CREDENTIALS = credentials('0706e217-7191-4f12-aa04-4cec85ea558f')
    }

    stages {
        stage ('checkout') {
            steps {
                git credentialsId: 'aa2316bf-0c06-40c6-921f-9e551ed41f6c',
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
                docker.withRegistry('https://index.docker.io/v1/', '0706e217-7191-4f12-aa04-4cec85ea558f') {
        echo 'Push to repo'
        sh 'docker push gowri9493/cicd-e2e:${IMAGE_TAG}'
                }
       }
        }
        }
    
    stage ('Checkout K8S manifest SCM') {
        steps {
            git credentialsId: 'githubcred',
            url: 'https://github.com/gowri9493/cicd-demo-manifests-repo.git',
            branch: 'main'
        }
    }
        stage ('update K8S manifest and push to repo') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'githubcred', passwordVariable: 'GIT_TOKEN',
                    usernameVariable: 'GIT_USERNAME' )]) {
                    sh '''
                        cat deploy.yaml
                        ls -l deploy.yaml
                        sed -i "s/gowri9493\\/cicd-e2e:v1/gowri9493\\/cicd-e2e:${IMAGE_TAG}/g" deploy.yaml
                        cat deploy.yaml
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
}
