pipeline {
    agent any
    
    tools {
        nodejs 'node'
    }

    stages {
        stage('Preparation') {
            steps {
                script {
                    // Тармаққа байланысты атау мен портты осы жерде анықтаймыз
                    if (env.BRANCH_NAME == 'main') {
                        env.APP_NAME = 'nodemain'
                        env.PORT = '3000'
                    } else {
                        env.APP_NAME = 'nodedev'
                        env.PORT = '3001'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${env.APP_NAME}:v1.0 ."
            }
        }

        stage('Deploy') {
            steps {
                sh "docker stop ${env.APP_NAME} || true"
                sh "docker rm ${env.APP_NAME} || true"
                sh "docker run -d --name ${env.APP_NAME} -p ${env.PORT}:3000 ${env.APP_NAME}:v1.0"
            }
        }
    }
}
