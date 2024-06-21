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
        sh ' docker push gowri9493/cicd-e2e:${BUILD_NUMBER} '
                }
       }
        }
        }
    }
    }
