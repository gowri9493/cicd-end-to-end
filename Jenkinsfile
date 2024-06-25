pipeline {
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}" 
        DOCKERHUB_CREDENTIALS = credentials["dockerhub"] 
            }

            stages {
                stage ('checkout') {
                    steps {
                        git credentialsId: "githubcredentials",
                        url: "https://github.com/gowri9493/cicd-end-to-end.git",
                        branch: "main"
                    }
                }
            }
}
