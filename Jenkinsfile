pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-south-2'
        S3_BUCKET = 'krishna-codedeploy-bucket'
        APPLICATION_NAME = 'kk-sample-microservice-app'
        DEPLOYMENT_GROUP = 'kk-sample-microservice-dg'
    }

    stages {

        stage('Clone Repository') {
    steps {
        git branch: 'main',
        url: 'https://github.com/krishnakumar-31/sample-micro-deploy.git'
    }
  }

        stage('Create Deployment Package') {
            steps {
                sh 'zip -r deployment.zip .'
            }
        }

        stage('Upload to S3') {
            steps {
                sh '''
                aws s3 cp deployment.zip s3://$S3_BUCKET/
                '''
            }
        }

        stage('Deploy to CodeDeploy') {
            steps {
                sh '''
                aws deploy create-deployment \
                --application-name $APPLICATION_NAME \
                --deployment-group-name $DEPLOYMENT_GROUP \
                --s3-location bucket=$S3_BUCKET,bundleType=zip,key=deployment.zip
                '''
            }
        }
    }
}
