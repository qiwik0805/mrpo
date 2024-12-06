pipeline {
    agent any

    environment {
        MY_SECRET_TEXT = credentials('my-secret-text')
    }

    stages {
        stage("Build") {
            steps {
                echo "Сборка приложения... Secret: ${MY_SECRET_TEXT}"
            }
        }

        stage("Test") {
            steps {
                echo "Тестирование приложения..."
            }
        }

        stage("Deploy") {
            steps {
                echo "Развёртывание приложения..."
            }
        }

        stage("Deploy to Staging") {
            steps {
                echo "Deploying to Staging..."
                sh "./deploy-staging.sh"
            }
        }

        stage("Deploy to Production") {
            when {
                expression {
                    input message: "Выполнить деплой на Production?", ok: "Да"
                }
            }
            steps {
                echo "Deploying to Production..."
                sh "./deploy-production.sh"
            }
        }
    }

    post {
        always {
            deleteDir()
        }
        failure {
            echo "Это будет выполняться, если задача провалилась"
            mail to: "gladiolud64english@gmail.com",
            subject: "${env.JOB_NAME} – Сборка № ${env.BUILD_NUMBER} провалилась",
            body: "Для получения дополнительной информации о провале пайплайна, проверьте консольный вывод по адресу ${env.BUILD_URL}"
        }
    }
}
