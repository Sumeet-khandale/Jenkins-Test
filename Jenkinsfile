pipeline {
    agent {
        docker {
            // ✅ Use a valid Maven + Java 17 image
            image 'maven:3.9.9-eclipse-temurin-17'
            args '-v /root/.m2:/root/.m2' // Cache Maven dependencies
        }
    }

    environment {
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=false"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git 'https://github.com/Sumeet-khandale/Jenkins-Test.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
            }
        }

        stage('Package Info') {
            steps {
                echo 'Listing target artifacts...'
                sh 'ls -lh target/'
            }
        }

        stage('Deploy (Optional)') {
            when {
                expression { fileExists('target/*.jar') }
            }
            steps {
                echo 'Starting Spring Boot app inside Jenkins agent...'
                sh 'nohup java -jar target/*.jar > app.log 2>&1 &'
                echo 'App started successfully (check logs with: tail -f app.log)'
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed — check console output for details.'
        }
    }
}
