/*                     def tempDir = sh(script: 'mktemp -d', returnStdout: true).trim()
                    dir(tempDir) {
                        sh 'echo "Building the project..."'
                        sh 'whoami && pwd'
                    } */
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
                    sh "sudo docker run -d -p ${params.PORT}:8000 --name art_explorer art_explorer"
                }
            }
        }
    }
}