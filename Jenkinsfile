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
            steps {
                step([$class: 'AWSEBDeploymentBuilder', credentialId: 'aws-cred',
                awsRegion: 'us-east-1', applicationName: 'my-application', environmentName: 'develop', keyPrefix: 'dev', sleepTime: 5,
                bucketName: 'elasticbeanstalk-us-east-1-855171129788', rootObject: 'SampleWebApp/target/SampleWebApp-1.0.null.war', versionLabelFormat: '$BUILD_NUMBER'])
                
            }
        }
        stage('Deploy to Prod') {
             when {
               branch "master"
                }
            steps {
                step([$class: 'AWSEBDeploymentBuilder', credentialId: 'aws-cred',
                awsRegion: 'us-east-1', applicationName: 'my-application', environmentName: 'production', keyPrefix: 'prod', sleepTime: 5,
                bucketName: 'elasticbeanstalk-us-east-1-855171129788', rootObject: 'SampleWebApp/target/SampleWebApp-1.0.null.war', versionLabelFormat: '$BUILD_NUMBER'])
                
            }
        }
    }
}
