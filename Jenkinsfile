pipeline {
    agent any
    stages {
        stage("Build") {
            environment {
                DB_HOST = ("laravel1.cwb1jzpmjmgj.ap-south-1.rds.amazonaws.com")
                DB_DATABASE = ("volt")
                DB_USERNAME = ("admin")
                DB_PASSWORD = ("12345678")
            }
            steps {
                sh 'php --version'
                sh 'composer install'
                sh 'composer --version'
                sh 'cp .env.example .env'
                sh 'echo DB_HOST=${DB_HOST} >> .env'
                sh 'echo DB_USERNAME=${DB_USERNAME} >> .env'
                sh 'echo DB_DATABASE=${DB_DATABASE} >> .env'
                sh 'echo DB_PASSWORD=${DB_PASSWORD} >> .env'
                sh 'php artisan key:generate'
                sh 'cp .env .env.testing'
                sh 'php artisan migrate'
            }
        }
        stage("Docker build") {
            steps {
                sh "docker build -t vrpawar86/volt ."
            }
        }
        stage("Docker push") {
            environment {
                DOCKER_USERNAME = ("vrpawar86")
                DOCKER_PASSWORD = ("1211122111Vi")
            }
            steps {
                sh "docker login --username ${DOCKER_USERNAME} --password ${DOCKER_PASSWORD}"
                sh "docker push vrpawar86/volt"
            }
        }
        stage("Deploy to staging") {
            steps {
                sh "docker run -d --rm -p 80:80 --name laravel8cd vrpawar86/volt"
            }
        }
    }
}
