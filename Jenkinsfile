def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
]
def getBuildUser() {
    return currentBuild.rawBuild.getCause(Cause.UserIdCause).getUserId()
}
pipeline {
    environment {
        //This variable need be tested as string
        doError = '0'
        BUILD_USER = ''
    }

    agent any
      stages {
        stage ('Compile Stage') {

            steps {
                slackSend (channel: '#deploy',color: '#FFFF00', message: "COMPILE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
                withMaven(maven : 'apache-maven-3.6.0') {
                    sh ' mvn clean compile'
                }
            }
        }
        stage ('Testing Stage') {

            steps {
                withMaven(maven : 'apache-maven-3.6.0') {
                    sh 'mvn test'
                }
            }
        }
          
       
          
        
          
         stage ('SonarQube analysis') {
             environment {
             def scannerHome = tool 'sonarscanner'
             }
             steps {
                withSonarQubeEnv('jenkinsonar') { 
                sh 'mvn clean package sonar:sonar'
             }
            }
         }
          stage ('Building stage') {
            steps {
                withMaven(maven : 'apache-maven-3.6.0') {
                    sh 'mvn install'
                }
            }
        }
          
          stage ('Push to Artifactory') {

            steps {
                withMaven(maven : 'apache-maven-3.6.0') {
                    sh 'mvn deploy'
                }
            }
        }
       stage('Error') {
            // when doError is equal to 1, return an error
            when {
                expression { doError == '1' }
            }
            steps {
                echo "Failure :("
                error "Test failed on purpose, doError == str(1)"
            }
        }
        stage('Success') {
            // when doError is equal to 0, just print a simple message
            when {
                expression { doError == '0' }
            }
            steps {
                echo "Success :)"
                 }
             }   
         }

}

