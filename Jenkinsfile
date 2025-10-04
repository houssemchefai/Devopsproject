pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'      // Must match the name configured in Jenkins > Global Tool Configuration
        maven 'M2_HOME'      // Must match the Maven name in Jenkins
    }

    environment {
        DOCKER_IMAGE = "houssemchefai/student-management:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📥 Cloning the GitHub repository...'
                git branch: 'main', url: 'https://github.com/houssemchefai/Devopsproject.git'
            }
        }

        stage('Build') {
            steps {
                echo '🏗️ Building the project with Maven...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Running unit tests...'
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo '🐳 Building Docker image...'
                    if (fileExists('Dockerfile')) {
                        sh "docker build -t ${DOCKER_IMAGE} ."
                        echo "✅ Docker image ${DOCKER_IMAGE} built successfully!"
                    } else {
                        echo '⚠️ No Dockerfile found. Skipping Docker build.'
                    }
                }
            }
        }

        stage('Run Container (Optional)') {
            when {
                expression { fileExists('Dockerfile') }
            }
            steps {
                script {
                    echo '🚀 Running Docker container for testing...'
                    sh "docker run -d -p 8080:8080 ${DOCKER_IMAGE}"
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully — Build, Test, and Docker steps passed!'
        }
        failure {
            echo '❌ Pipeline failed. Check the Jenkins logs for details.'
        }
        always {
            echo '📋 Pipeline finished. Cleaning up temporary files...'
            sh 'docker ps -a'
        }
    }
}
