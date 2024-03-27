def frontendImage="xmacple/frontend"
def backendImage="xmacple/backend"
def backendDockerTag=""
def frontendDockerTag=""
def dockerRegistry=""
def registryCredentials="dockerhub"

pipeline {
    parameters {
        string description: 'Backend docker image tag', name: 'backendDockerTag'
        string description: 'Frontend docker image tag', name: 'frontendDockerTag'
    }

    agent {
        label 'agent'
    }
    stages {
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
    }
}