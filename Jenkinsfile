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

        stage('Deploy to Staging') {
            when {
               branch "staging"
                }
            steps {
                step([$class: 'AWSEBDeploymentBuilder', credentialId: 'aws-cred',
                awsRegion: 'us-east-2', applicationName: 'tomcat-app', environmentName: 'staging-env', keyPrefix: 'stage', sleepTime: 5,
                bucketName: 'elasticbeanstalk-us-east-2-855171129788', rootObject: 'SampleWebApp/target/SampleWebApp-1.0.null.war', versionLabelFormat: 'staging-$BUILD_NUMBER'])
                
            }
        }

        stage('Deploy to Prod') {
             when {
               branch "master"
                }
            steps {
                step([$class: 'AWSEBDeploymentBuilder', credentialId: 'aws-cred',
                awsRegion: 'us-east-2', applicationName: 'tomcat-app', environmentName: 'production1', keyPrefix: 'prod', sleepTime: 5,
                bucketName: 'elasticbeanstalk-us-east-2-855171129788', rootObject: 'SampleWebApp/target/SampleWebApp-1.0.null.war', versionLabelFormat: 'prod-$BUILD_NUMBER'])
                
            }
        }
    }
}
