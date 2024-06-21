pipeline {
    agent any 

    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage ('checkout') {
            steps {
                git credentialsId: '4a313cc0-0775-4139-83df-05384f27d375',
                url: 'https://github.com/gowri9493/cicd-end-to-end.git',
                branch: 'main'
            }
        }
    }
    stage ('Build Image') {
        echo "Build docker Image"
    }
}
