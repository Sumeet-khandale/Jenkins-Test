pipeline {
    agent none

    stages {
        stage('Build in Docker') {
            agent {
                docker {
                    image 'maven:3.9.9-eclipse-temurin-17'
                    // Mount Jenkins workspace manually with correct Linux path
                    args '-v /c/ProgramData/Jenkins/.jenkins/workspace/jenkins:/workspace -w /workspace'
                    reuseNode true
                }
            }
            steps {
                echo '📦 Cloning repository...'
                git branch: 'master', url: 'https://github.com/Sumeet-khandale/Jenkins-Test.git'

                echo '⚙️ Building project inside Docker container...'
                sh 'mvn clean package -DskipTests'
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
