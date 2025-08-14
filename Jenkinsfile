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
                        docker run -d -p 80:80 --name jenkins \\
                            $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG

                        sleep 10
                    '''
                }
            }
        }
        //         stage('Docker Push') {
        //     environment {
        //         DOCKER_PASS = credentials("DOCKER_HUB_PASS")
        //     }
        //     steps {
        //         script {
        //             sh '''
        //                 docker login -u $DOCKER_ID -p $DOCKER_PASS
        //                 docker push $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG
        //             '''
        //         }
        //     }
        // }

        // stage('Deploy to Dev') {
        //     environment {
        //         KUBECONFIG = credentials('config')  // Jenkins secret file with your kubeconfig
        //     }
        //     steps {
        //         script {
        //             sh '''
        //                 # Prepare kubeconfig directory & write the kubeconfig file from Jenkins secret
        //                 rm -rf .kube && mkdir .kube
        //                 cat $KUBECONFIG > .kube/config

        //                 # Replace localhost address with the actual cluster IP in kubeconfig
        //                 sed -i 's|https://127.0.0.1:6443|https://172.30.189.142:6443|g' .kube/config

        //                 # Verify minimal kubeconfig content & current context
        //                 echo "== kubeconfig minimal content =="
        //                 kubectl --kubeconfig=.kube/config config view --minify

        //                 echo "== current context =="
        //                 kubectl --kubeconfig=.kube/config config current-context

        //                 # Confirm connectivity - get cluster nodes
        //                 echo "== checking connectivity =="
        //                 kubectl --kubeconfig=.kube/config get nodes

        //                 # Create namespace 'dev' if it doesn't exist
        //                 kubectl --kubeconfig=.kube/config create namespace dev --dry-run=client -o yaml | \\
        //                     kubectl --kubeconfig=.kube/config apply -f -

        //                 # Prepare Helm values.yaml with Docker tag
        //                 cp charts/values.yaml values.yml
        //                 sed -i "s+tag:.*+tag: ${DOCKER_TAG}+g" values.yml

        //                 # Deploy or upgrade app with Helm using kubeconfig
        //                 helm upgrade --install app charts --values values.yml --namespace dev --kubeconfig=.kube/config
        //             '''
        //         }
        //     }
        // }

        stage('List Workspace') {
            steps {
                sh 'ls -la'
            }
        }
    }
}
