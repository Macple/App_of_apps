def frontendImage="xmacple/frontend"
def backendImage="xmacple/backend"
def backendDockerTag=""
def frontendDockerTag=""
def dockerRegistry=""
def registryCredentials="Dockerhub"

pipeline {
    parameters {
        string description: 'Backend docker image tag', name: 'backendDockerTag'
        string description: 'Frontend docker image tag', name: 'frontendDockerTag'
    }

    agent {
        label 'agent'
    }

    environment {
        PIP_BREAK_SYSTEM_PACKAGES = 1
    }

    stages {
        stage('Clean running containers') {
            steps {
                sh "docker rm -f frontend backend"
            }
        }
        stage('Get Code') {
            steps {
                checkout scm
            }
        }
        stage('Adjust version') {
            steps {
                script{
                    backendDockerTag = params.backendDockerTag.isEmpty() ? "latest" : params.backendDockerTag
                    frontendDockerTag = params.frontendDockerTag.isEmpty() ? "latest" : params.frontendDockerTag
                   
                    currentBuild.description = "Backend: ${backendDockerTag}, Frontend: ${frontendDockerTag}"
                }
            }
        }
        stage('Deploy application') {
            steps {
                script {
                    withEnv(["FRONTEND_IMAGE=$frontendImage:$frontendDockerTag", 
                             "BACKEND_IMAGE=$backendImage:$backendDockerTag"]) {
                       docker.withRegistry("$dockerRegistry", "$registryCredentials") {
                            sh "docker-compose up -d"
                        }
                    }
                }
            }
        }
        stage('Selenium tests') {
            steps {
                sh "pip3 install -r test/selenium/requirements.txt"
                sh "python3 -m pytest test/selenium/frontendTest.py"
            }
        }
    }

    post {
        always {
            sh "docker-compose down"
            cleanWs()
        }
    }
}