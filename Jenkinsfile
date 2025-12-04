@Library('Shared') _

pipeline {
    agent { label 'dev' }

    environment {
        # MySQL environment variables
        DB_NAME = 'test_db'
        DB_USER = 'root'
        DB_PASSWORD = 'root'
        DB_HOST = 'db_cont'   // Replace with your MySQL host/container name
        DB_PORT = '3306'
    }

    stages {
        stage("Code clone") {
            steps {
                sh "whoami"
                clone("https://github.com/LondheShubham153/django-notes-app.git","main")
            }
        }

        stage("Code Build") {
            steps {
                dockerbuild("notes-app","latest")
            }
        }

        stage("Push to DockerHub") {
            steps {
                dockerpush("dockerHubCreds","notes-app","latest")
            }
        }

        stage("Deploy") {
            steps {
                // Run your deploy function with MySQL env variables
                deploy([
                    "DB_NAME=${DB_NAME}",
                    "DB_USER=${DB_USER}",
                    "DB_PASSWORD=${DB_PASSWORD}",
                    "DB_HOST=${DB_HOST}",
                    "DB_PORT=${DB_PORT}"
                ])
            }
        }
    }
}
