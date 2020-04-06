pipeline {
    agent any
      stages {
        stage ('Compile Stage') {

            steps {
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
          
       
          
        stage ('Install Stage') {
            steps {
                withMaven(maven : 'apache-maven-3.6.0') {
                    sh 'mvn install'
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
          stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "jenkinsartifact"
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "jenkinsartifact",
                    releaseRepo: "libs-release-local",
                    snapshotRepo: "libs-snapshot-local"
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "jenkinsartifact",
                    releaseRepo: "jenkins-release",
                    snapshotRepo: "jenkins-snapshot"
                )
            }
        }
          stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: 'apache-maven-3.6.0', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
            }
        }
         }
}
