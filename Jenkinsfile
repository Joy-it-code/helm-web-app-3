pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
    }

    options {
        skipDefaultCheckout(true) // Prevents checkout unless you define it explicitly
    }

    stages {
        stage('Checkout') {
            when {
                anyOf {
                    branch 'main'
                
                }
            }
            steps {
                checkout scm
            }
        }

        stage('Deploy with Helm') {
            when {
                anyOf {
                    branch 'main'
                   
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        export AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
                        export AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY
                        aws eks --region us-east-1 update-kubeconfig --name my-eks-cluster
                        helm upgrade --install my-webapp ./webapp --namespace default
                    '''
                }
            }
        }
    }
}
