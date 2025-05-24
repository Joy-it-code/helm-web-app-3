pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        ECR_REGISTRY = '586794450782.dkr.ecr.us-east-1.amazonaws.com'
        IMAGE_NAME = 'my-webapp'
        IMAGE_TAG = 'latest'
        FULL_IMAGE_NAME = "${ECR_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}"
        EKS_CLUSTER_NAME = 'my-eks-cluster'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Build and Push to ECR') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'aws-cred',
                    usernameVariable: 'AWS_ACCESS_KEY_ID',
                    passwordVariable: 'AWS_SECRET_ACCESS_KEY'
                )]) {
                    withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}"]) {
                        sh '''
                            echo "Checking AWS identity..."
                            aws sts get-caller-identity

                            echo "Logging into ECR..."
                            aws ecr get-login-password --region $AWS_DEFAULT_REGION | \
                                docker login --username AWS --password-stdin $ECR_REGISTRY

                            echo "Building Docker image..."
                            docker build -t $FULL_IMAGE_NAME .

                            echo "Pushing Docker image to ECR..."
                            docker push $FULL_IMAGE_NAME

                            echo "Cleaning up local image..."
                            docker rmi $FULL_IMAGE_NAME || true
                        '''
                    }
                }
            }
        }

        stage('Deploy with Helm to EKS') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'aws-cred',
                    usernameVariable: 'AWS_ACCESS_KEY_ID',
                    passwordVariable: 'AWS_SECRET_ACCESS_KEY'
                )]) {
                    withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}"]) {
                        sh '''
                            echo "Configuring access to EKS..."
                            aws eks --region $AWS_DEFAULT_REGION update-kubeconfig --name $EKS_CLUSTER_NAME

                            echo "Deploying with Helm..."
                            helm upgrade --install my-webapp ./webapp \
                                --set image.repository=$ECR_REGISTRY/$IMAGE_NAME \
                                --set image.tag=$IMAGE_TAG \
                                --namespace default
                        '''
                    }
                }
            }
        }
    }
}
