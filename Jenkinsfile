pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/kenkool23/NewProject4.git'
            }
        }
        
        stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean package'
            }
        }

        stage('Deploy to Dev') {
            when {
               branch "develop"
                }
            steps {
                step([$class: 'AWSEBDeploymentBuilder', credentialId: 'aws-cred',
                awsRegion: 'us-east-2', applicationName: 'just-app', environmentName: 'develop-env', keyPrefix: 'dev', sleepTime: 5,
                bucketName: 'elasticbeanstalk-us-east-2-855171129788', rootObject: 'SampleWebApp/target/SampleWebApp-1.0.null.war', versionLabelFormat: 'dev-$BUILD_NUMBER'])
                
            }
        }
        stage('Deploy to Prod') {
             when {
               branch "master"
                }
            steps {
                step([$class: 'AWSEBDeploymentBuilder', credentialId: 'aws-cred',
                awsRegion: 'us-east-2', applicationName: 'just-app', environmentName: 'production', keyPrefix: 'prod', sleepTime: 5,
                bucketName: 'elasticbeanstalk-us-east-2-855171129788', rootObject: 'SampleWebApp/target/SampleWebApp-1.0.null.war', versionLabelFormat: 'prod-$BUILD_NUMBER'])
                
            }
        }
    }
}
