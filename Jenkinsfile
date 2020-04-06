pipeline {
    agent any
      stages {
        stage ('Compile Stage') {

            steps {
                withMaven(maven : 'apache-maven-3.6.0') {
                    sh 'mvn clean compile'
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
          
       
          
        stage ('Install Stage') {
            steps {
                withMaven(maven : 'apache-maven-3.6.0') {
                    sh 'mvn install'
                }
            }
        }
          
          stage ('SonarQube analysis') {
            def scannerHome = tool 'SonarScanner 4.3.0';
            withSonarQubeEnv('jenkinsonar') { // If you have configured more than one global server connection, you can specify its name
            sh "${scannerHome}/bin/sonar-scanner"
    }
  }
    }
    
}
