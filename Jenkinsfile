pipeline {
    agent any

    stages {
        stage('Build in Docker') {
            steps {
                echo '📦 Cloning repository...'
                git branch: 'master', url: 'https://github.com/Sumeet-khandale/Jenkins-Test.git'

                echo '⚙️ Building project inside Docker container...'
                bat '''
                docker run --rm ^
                    -v "%cd%:/workspace" ^
                    -w /workspace ^
                    maven:3.9.9-eclipse-temurin-17 ^
                    mvn clean package -DskipTests
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build Successful!'
        }
        failure {
            echo '❌ Build Failed — check logs above.'
        }
    }
}
