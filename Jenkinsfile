pipeline {

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('2849ac02-88bd-4014-be50-6739a927ac93')
		AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id')
  		AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
		ARTIFACT_NAME = 'Dockerrun.aws.json'
		AWS_S3_BUCKET = 'maha-belt2-artifacts-123456'
		AWS_EB_APP_NAME = 'runway-b2d3'
        AWS_EB_ENVIRONMENT_NAME = 'Runwayb2d3-env'
        AWS_EB_APP_VERSION = "${BUILD_ID}"
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build -t 2160003120/runaay:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push 2160003120/runaay:latest'
			}
		}

        stage('Deploy') {
            steps {
                sh 'aws configure set region us-west-2	'
                sh 'aws elasticbeanstalk create-application-version --application-name $AWS_EB_APP_NAME --version-label $AWS_EB_APP_VERSION --source-bundle S3Bucket=$AWS_S3_BUCKET,S3Key=$ARTIFACT_NAME'
                sh 'aws elasticbeanstalk update-environment --application-name $AWS_EB_APP_NAME --environment-name $AWS_EB_ENVIRONMENT_NAME --version-label $AWS_EB_APP_VERSION'
            }
	    }   
    
        stage ("Logout") {
            steps {
                sh 'docker logout'
            }
        }
    }

}