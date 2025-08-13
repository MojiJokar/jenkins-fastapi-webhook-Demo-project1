pipeline {
    environment {
        DOCKER_ID = "maxjokar2020"         // Your Docker Hub ID
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

        stage('Docker Run') {
            steps {
                script {
                    sh '''
                        # 🚀 Remove any old container named "jenkins" before starting a new one
                        docker rm -f jenkins || true

                        # Run the new container
                        docker run -d -p 80:80 --name jenkins \
                            $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG

                        sleep 10
                    '''
                }
            }
        }

        stage('List Workspace') {
            steps {
                sh 'ls -la'
            }
        }
    }
}
