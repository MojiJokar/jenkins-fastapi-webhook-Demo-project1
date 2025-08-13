pipeline { 
  environment { 
    DOCKER_ID = "maxjokar2020" // Your Docker Hub ID 
    DOCKER_IMAGE = "datascientestapi" 
    DOCKER_TAG = "v.${BUILD_ID}.0" 
  } 
    agent any
    stages {
        stage('Check Environment') {
            steps {
                echo "Jenkins pipeline is running."
                sh 'whoami'
                sh 'env'
            }
        }
        stage('List Workspace') {
            steps {
                sh 'ls -la'
            }
        }
    }
}

