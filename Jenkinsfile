pipeline {
    agent any
 
    options {
        skipDefaultCheckout(true)
    }

    tools { nodejs "node" }
 

    stages {
        stage('Git') {
            steps {
                echo 'Checking out the Git version control ...'
                checkout scm
            }
        }
        stage('Build image') {
            steps {
                echo 'Bulding the image'
                sh '''
                set -x
                make docker-build
                '''
            }
        }
        stage('Run image') {
            steps {
                echo 'Running the image'
                sh '''
                set -x
                make docker-run
                '''
            }
        }
        stage('Unit tests') {
            steps {
                echo "Running unit tests"
                sh '''
                set -x
                npm run test
                '''
            }
        }
        stage('Endpoint tests') {
            steps {
                echo "Testing the service"
                curl --silent --fail "curl http://localhost:3000" >/dev/null
            }
        }
    }
    post {
        always {
            sh '''
            set -x
            make docker-stop
            '''
        }
    }
}