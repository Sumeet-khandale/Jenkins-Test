pipeline {
    agent {
        docker {
            image 'maven:3.9.9-eclipse-temurin-17'
            args '-v /root/.m2:/root/.m2'
        }
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
                echo 'Building project...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Post-Build') {
            steps {
                echo '✅ Build successful — artifact ready in target/*.jar'
            }
        }
    }

    post {
        failure {
            echo '❌ Build failed — check logs above.'
        }
    }
}
