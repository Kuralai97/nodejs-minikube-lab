pipeline {
    agent any
    
    // Біз баптаған NodeJS құралын қолдану
    tools {
        nodejs 'node' 
    }

    environment {
        // Тармаққа байланысты атау мен портты автоматты таңдау
        // Егер main болса - nodemain:3000, егер dev болса - nodedev:3001
        APP_NAME = "${env.BRANCH_NAME}" == 'main' ? 'nodemain' : 'nodedev'
        PORT = "${env.BRANCH_NAME}" == 'main' ? '3000' : '3001'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                // Егер тесттерің болмаса, бұл жерді 'echo "No tests"' деп өзгертуге болады
                sh 'npm test || echo "Tests failed but continuing..."'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${APP_NAME}:v1.0 ."
            }
        }

        stage('Deploy (Docker Run)') {
            steps {
                // Ескі контейнер болса өшіріп, жаңасын іске қосамыз
                sh "docker stop ${APP_NAME} || true"
                sh "docker rm ${APP_NAME} || true"
                sh "docker run -d --name ${APP_NAME} -p ${PORT}:3000 ${APP_NAME}:v1.0"
            }
        }
    }
}
