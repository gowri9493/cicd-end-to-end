pipeline {
    agent any 

    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage ('checkout') {
            steps {
                url: 'https://github.com/gowri9493/cicd-end-to-end.git',
                git credentialsId: '4a313cc0-0775-4139-83df-05384f27d375',
                branch: 'main'
            }
        }
    }
}
