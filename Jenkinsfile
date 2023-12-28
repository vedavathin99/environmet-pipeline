pipeline {

agent none

parameters {
  choice choices: ['dev', 'qa'], name: 'Environment'
  string defaultValue: 'veda', name: 'usr'
}

environment {
		envi = "${params.Environment}"
  		user = "${params.usr}"
}

	stages {		
		stage('stage1') {
				agent { label 'sonar' }
    				steps {
      					echo "Hi this is: ${user}"
					sh 'echo "Build URL: ${BUILD_URL}"'
    					}
  				}
		stage('stage2') {
				agent { label 'sonar' }
    				steps {
      					echo "Deployment Environment: ${envi}"
					sh 'echo "Jenkins URL: ${JENKINS_URL}"'
    					}
  				}
  		stage('stage3') {
				agent { label 'tomcat' }
				when {
				allOf {
                			expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
				}
            			}
    				steps {
      					sh 'echo "Branch Name: ${GIT_BRANCH}"'
    				}
  				}
    		stage('stage4') {
				when {
                		expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            			}
				agent { label 'tomcat' }
    				steps {
      					sh 'echo "Build ID: ${BUILD_ID}"'
					build 'demo'
    					}
				}
	
	}

		post {
  		always {
    				emailext body: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS: Check console output at $BUILD_URL to view the results.', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'vedavathi0892@gmail.com'
			}
		}
	
}
