pipeline {
    agent any

    environment {
        DOCKER_ID = "maxjokar2020"                   // Your Docker Hub ID
        DOCKER_IMAGE = "jenkins-fastapi-demo-project"
        // DOCKER_TAG = "v.${BUILD_ID}.0"
        DOCKER_TAG = "v.4.0"                         // Use fixed version tag to avoid Jenkins versioning issues
    }

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

        stage('Docker Push') {
            environment {
                DOCKER_PASS = credentials("DOCKER_HUB_PASS")
            }
            steps {
                script {
                    sh '''
                        docker login -u $DOCKER_ID -p $DOCKER_PASS
                        docker push $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG
                    '''
                }
            }
        }

        stage('Deployment in dev') {
            environment {
                KUBECONFIG = credentials("minikube-kubeconfig") // Jenkins secret file credential with kubeconfig
            }
            steps {
                script {
                    sh '''
                        rm -Rf ~/.kube
                        mkdir -p ~/.kube
                        cat $KUBECONFIG > ~/.kube/config
                        chmod 600 ~/.kube/config
                        export KUBECONFIG=~/.kube/config

                        # Verify cluster connectivity
                        kubectl get nodes

                        cp fastapi/values.yaml values.yml
                        cat values.yml

                        sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
                        helm upgrade --install app fastapi --values=values.yml --namespace dev
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
