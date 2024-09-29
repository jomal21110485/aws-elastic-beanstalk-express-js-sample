pipeline {
    agent {
        docker {
            image 'node:16'
            args '-u root'
        }
    }

    environment {
        SNYK_TOKEN = credentials('snyk-token')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm // Check out the source code
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Starting to Install Project Dependencies using NPM...'
                    sh 'npm install --save'
                    echo 'Dependencies Installed Successfully.'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    echo 'Starting to Run Project Tests...'
                    sh 'npm test'
                    echo 'Tests Completed Successfully.'
                }
            }
        }

        stage('Snyk Security Scan') {
            steps {
                script {
                    echo 'Starting Snyk Security Scan...'
                    sh 'npm install -g snyk'
                    echo 'Snyk Installed Successfully.'
                    sh 'snyk auth $SNYK_TOKEN'
                    echo 'Running Snyk Security Scan with Severity Threshold Set to High...'
                    def result = sh(script: 'snyk test --severity-threshold=high', returnStatus: true)
                    if (result != 0) {
                        error "Critical vulnerabilities found! Halting the pipeline."
                    }
                    echo 'Snyk Scan Completed. Check for Any Critical Vulnerabilities.'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline Execution Finished. Check the Above Steps for Results.'
        }
        failure {
            echo 'Pipeline Execution Failed. Please Review the Logs for Details.'
        }
    }
}
