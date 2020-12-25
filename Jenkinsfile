pipeline {
   agent any
	stages {
      stage('Git Checkout') {
         steps {
            git 'https://github.com/kvmohan/mediclaim.git'
		}
	}
		
	/*stage('Build') {
		steps {
			withSonarQubeEnv('sonar') {
				sh '/opt/apache-maven-3.6.3/bin/mvn clean verify sonar:sonar -Dmaven.test.skip=true'
			}
		}
	}
	stage("Quality Gate") {
            steps {
              timeout(time: 2, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
          }*/
	stage ('Deploy') {
		steps {
			sh '/opt/apache-maven-3.6.3/bin/mvn clean install -Dmaven.test.skip=true'
		}
	}
	stage ('Release') {
		steps {
			sh 'export JENKINS_NODE_COOKIE=dontkillme ;nohup java -jar $WORKSPACE/target/*.jar &'
		}
	}
/*	stage ('DB Migration') {
		steps {
			sh '/opt/apache-maven-3.6.3/bin/mvn clean flyway:migrate'
		}
	}*/
}
	/*post {
        always {
            emailext body: "${currentBuild.currentResult}: Project Name : ${env.JOB_NAME} Build ID : ${env.BUILD_NUMBER}\n\n Approval Link :  ${env.BUILD_URL}", recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
        }
    }*/

	stage (confirmation)
       {
           steps{
            mail (to: 'vamsikvm@gmail.com',
            subject: "Job '${env.JOB_BASE_NAME}' (${env.BUILD_NUMBER}) is waiting for input",
            body: """STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'
            Please go to console output of ${env.BUILD_URL}console to approve or Reject .""");
            }
       }
}
