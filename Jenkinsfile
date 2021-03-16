pipeline {
        agent any
        tools
    {
       maven "Maven3.6.3"
       jdk 'jdk8'
    }
        stages {
           // Building web app using Maven build	
           stage("Build Web App") {
                   steps {
                           sh 'mvn -Dmaven.test.failure.ignore=true install'
                //buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
                }  
           }
          stage("build & SonarQube analysis") {
              steps {
              withSonarQubeEnv(credentialsId: 'adminsonar-token', installationName: 'sonarqube') {
                sh '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn -Dsonar.test.exclusions=**/test/java/servlet/createpage_junit.java -Dsonar.login=admin -Dsonar.password=sonar -Dsonar.tests=. -Dsonar.inclusions=**/test/java/servlet/createpage_junit.java -Dsonar.sources=. sonar:sonar -Dsonar.host.url=http://34.83.65.36:9000'
                // sh 'mvn clean package sonar:sonar'
              }
            }
          }
          stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
        }
      }

