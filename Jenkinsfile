pipeline {
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}" 
        DOCKERHUB_CREDENTIALS = credentials["1b36eb31-0bec-4739-a402-2a035073ff53"] 
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
