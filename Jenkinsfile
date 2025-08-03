def buildNumber = Jenkins.instance.getItem('cicd-jenkins-hybrid-bean-stage').lastSuccessfulBuild.number

pipeline {
    agent any    
    environment {
        ARTIFACT_NAME = "vprofile-v${buildNumber}.war"
        AWS_S3_BUCKET = 'vprocicdbeanbucket2580'
        AWS_EB_APP_NAME = 'vproapp-bean-stage-jenkins'
        AWS_EB_ENVIRONMENT = 'Vproappbeanstage-env'
        AWS_EB_APP_VERSION = "${buildNumber}"
    }

    stages {
        stage('Deploy to stage bean') {
            steps {
                withAWS(credentials: 'awsbeancreds', region: 'us-east-1' ) {
                    sh 'aws elasticbeanstalk update-environment --application-name $AWS_EB_APP_NAME --environment-name $AWS_EB_ENVIRONMENT --version-label $AWS_EB_APP_VERSION'
                }
            }
        }
    }
}