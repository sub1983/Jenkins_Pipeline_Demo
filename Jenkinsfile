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
        stage("build & SonarQube analysis") {
            
            agent any
            steps {
              withSonarQubeEnv('My SonarQube Server') {
                sh 'mvn clean package sonar:sonar'
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
    }
}
