
pipeline {
    agent { label 'onkar' }

    stages {

        stage('Code Clone') {
            steps {
                sh 'whoami'
                git branch: 'main', url: 'https://github.com/onkar-1817/Django-notes-app.git'
            }
        }

        stage('Build and Test') {
            steps {
                sh 'docker build -t note-app-test-new .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerHub',
                        usernameVariable: 'dockerHubUser',
                        passwordVariable: 'dockerHubPass'
                    )
                ]) {
                    sh '''
                    docker tag note-app-test-new ${dockerHubUser}/note-app-test-new:latest
                    docker login -u ${dockerHubUser} -p ${dockerHubPass}
                    docker push ${dockerHubUser}/note-app-test-new:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker compose down
                docker compose up -d
                '''
            }
        }
    }
}
