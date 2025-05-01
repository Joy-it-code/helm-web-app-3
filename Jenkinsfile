pipeline {
    agent any

    stages {
        stage('Deploy with Helm') {
            steps {
                script {
                    sh 'helm upgrade --install my-webapp ./webapp --namespace default'
                }
            }
        }
    }
}
