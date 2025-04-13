pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'rozee01/tp3_devops'
        HELM_CHART_PATH = './tp3'
    }
    stages {
        stage('Cloner le dépôt') {
            steps {
                git 'https://github.com/rozee01/Lab3_Devops.git'
            }
        }
        stage('Construire l\'image Docker') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE% .'
            }
        }
        stage('Pousser l\'image Docker') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    bat 'echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin'
                    bat 'docker push %DOCKER_IMAGE%'
                }
            }
        }
        stage('Déployer avec Helm') {
            steps {
                bat 'helm upgrade --install mon-app %HELM_CHART_PATH%'
            }
        }
    }
}
