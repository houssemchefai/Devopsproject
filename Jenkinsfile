pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'          // Must match the name of your JDK in Jenkins Global Tool Config
        maven 'M2_HOME'        // Must match the name of your Maven in Jenkins
    }

    environment {
        DOCKER_IMAGE = "houssemchefai/student-management:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/houssemchefai/Devopsproject.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (fileExists('Dockerfile')) {
                        sh "docker build -t ${DOCKER_IMAGE} ."
                    } else {
                        echo 'No Dockerfile found, skipping Docker build'
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build, tests, and Docker image creation successful!'
        }
        failure {
            echo '❌ Something went wrong in the pipeline.'
        }
    }
}
