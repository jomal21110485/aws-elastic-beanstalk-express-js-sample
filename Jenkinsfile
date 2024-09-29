pipeline {
    agent {
        docker {
            image 'node:16'
            args '-u root'
        }
    }

    stages {
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
    }
    
    post {
        always {
            echo 'Pipeline Execution Finished. Check the Above Steps for Results.'
        }
    }
}
