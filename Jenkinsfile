pipeline {
    agent any
    stages {
        stage('Test - Deploy to k3s') {
            agent {
                docker {
                    image 'dtzar/helm-kubectl:latest'
                    args '-v /var/lib/jenkins/config:/var/lib/jenkins/config'
                }
            }
            steps {
                    echo 'Deploying using helm...'
                    sh 'export KUBECONFIG=/var/lib/jenkins/config && kubectl apply -n registry -f ./ --dry-run server'
            }
        }
        stage('Deploy to k3s') {
            when {
                expression {
                    env.BRANCH_NAME == 'master'
                }
            }
            agent {
                docker {
                    image 'dtzar//helm-kubectl:latest'
                    args '-v /var/lib/jenkins/config:/var/lib/jenkins/config'
                }
            }
            steps {
                    echo 'Deploying using helm...'
                    sh 'export KUBECONFIG=/var/lib/jenkins/config && kubectl apply -n registry -f ./'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}