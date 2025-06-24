pipeline {
    agent {
        label 'vagrant'
    }
    // paramètre pour le port de l'application
    parameters {
        string(name: 'PORT', defaultValue: '5000', description: "Port pour l'application")
    }
    stages {
        stage('Build') {
            steps {
                script {
                    sh "sudo docker build . -t art_explorer"
                }
            }
            post {
                failure {
                    echo 'Build stage failed. Sending notification or performing cleanup...'
                    sh 'curl https://discord.com/api/webhooks/1387160798354477056/VEaT1V1rhCAtU1fuqxNq3Q8ms2qi2R8auYdEKIkWdBRfVl2y3oNOn6PlFsLVAUklGtJH -d "{\"content\": \"Build stage failed for art_explorer!\"}"'
                }
            }
        }
        stage('Unit Tests') {
            steps {
                script {
                    sh 'sudo docker run --entrypoint=ash art_explorer -c "python -m pytest"'
                }
            }
        }
        stage('Unit Tests with Coverage') {
            steps {
                script {
                    sh 'sudo docker run --entrypoint=ash art_explorer -c "coverage run -m pytest && coverage report"'
                }
            }
        }
        stage('Launch app') {
            steps {
                script {
                    sh 'sudo docker rm -f art_explorer'
                    sh "sudo docker run -d -p ${params.PORT}:8000 --name art_explorer art_explorer"
                }
            }
        }
    }
}