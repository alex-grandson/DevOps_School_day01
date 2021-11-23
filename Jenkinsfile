pipeline {
    agent any
    triggers { pollSCM('H */4 * * 1-3') }
    stages {
        stage('Build') {
            steps {
                sh 'export GIT_COMMIT=$(git log -1 --format=%h)'
                sh 'docker build -t 285484/weather-app:master-$GIT_COMMIT .'
            }
        }
        stage('Publish master') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME_DOCKER', passwordVariable: 'PASSWORD_DOCKER')]) {
                    sh 'docker login -u $USERNAME_DOCKER -p $PASSWORD_DOCKER'
                    sh 'docker push 285484/weather-app:master-$GIT_COMMIT'
                }
            }
        }
        stage('Update :latest') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME_DOCKER', passwordVariable: 'PASSWORD_DOCKER')]) {
                    sh 'docker login -u $USERNAME_DOCKER -p $PASSWORD_DOCKER'
                    sh 'docker tag 285484/weather-app:master-$GIT_COMMIT 285484/weather-app:latest'
                    sh 'docker push 285484/weather-app:latest'
                }
            }
        }
        stage ('K8s deploy') {
            steps {
                echo 'Mock'
            }
        }
    }
    post {
        always {
            cleanWs()
            sh 'docker logout'
        }
    }
}