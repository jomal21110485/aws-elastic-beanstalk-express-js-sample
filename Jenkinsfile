pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                    sh 'docker run --rm -v $(pwd):/app -w /app node:16 npm install' // Run npm install in Docker
            }
        }
    }
}
