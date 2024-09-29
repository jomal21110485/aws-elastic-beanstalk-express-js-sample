pipeline {
    agent {
        // Use a Docker container with Node.js version 16
        docker {
            image 'node:16' 
            args '-u root' // Run the container as the root user
        }
    }

    environment {
        // Retrieve Snyk token from Jenkins credentials
        SNYK_TOKEN = credentials('snyk-token') // This is configured in Jenkins credentials
    }
    
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Log message indicating the start of dependency installation
                    echo 'Starting to Install Project Dependencies using NPM...'
                    // Install project dependencies using npm
                    sh 'npm install --save'
                    // Log message indicating successful installation
                    echo 'Dependencies Installed Successfully.'
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    // Log message indicating the start of running tests
                    echo 'Starting to Run Project Tests...'
                    // Execute tests defined in the project using npm
                    sh 'npm test'
                    // Log message indicating tests have been completed
                    echo 'Tests Completed Successfully.'
                }
            }
        }
        
        stage('Snyk Security Scan') {
            steps {
                script {
                    // Log message indicating the start of the Snyk security scan
                    echo 'Starting Snyk Security Scan...'
                    // Install the Snyk CLI globally
                    sh 'npm install -g snyk'
                    
                    // Uncomment this line if express is a new dependency
                    // sh 'npm install express@4.20.0 --save'
                    
                    // Log message indicating Snyk has been installed
                    echo 'Snyk Installed Successfully.'
                    
                    // Authenticate Snyk using the provided token
                    sh 'snyk auth $SNYK_TOKEN'
                    
                    // Log message indicating the start of the Snyk scan with a severity threshold
                    echo 'Running Snyk Security Scan with Severity Threshold Set to High...'
                    // Run the Snyk test and capture the exit status
                    def result = sh(script: 'snyk test --severity-threshold=high', returnStatus: true)
                    // If vulnerabilities are found, halt the pipeline
                    if (result != 0) {
                        error "Critical vulnerabilities found! Halting the pipeline."
                    }
                    // Log message indicating the Snyk scan is completed
                    echo 'Snyk Scan Completed. Check for Any Critical Vulnerabilities.'
                }
            }
        }
    }
    
    post {
        always {
            // Log message indicating the end of the pipeline execution
            echo 'Pipeline Execution Finished. Check the Above Steps for Results.'
        }
        failure {
            // Log message indicating that the pipeline has failed
            echo 'Pipeline Execution Failed. Please Review the Logs for Details.'
        }
    }
}
