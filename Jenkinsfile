pipeline {
    agent {
        docker {
            image 'maven:3.9.9-eclipse-temurin-17'
            args '-v /c/ProgramData/Jenkins/.jenkins/workspace:/workspace'
        }
    }
    environment {
        WORKSPACE_DIR = '/workspace/jenkins'
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git branch: 'master', url: 'https://github.com/Sumeet-khandale/Jenkins-Test.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building in ${WORKSPACE_DIR}"
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
