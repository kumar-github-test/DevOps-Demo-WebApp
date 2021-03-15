pipeline {
    agent any
 
   tools
    {
       maven "Maven3.6.3"
    }
 stages {
      stage('checkout') {
           steps {
                  git branch: 'master', url: 'https://github.com/kumar-github-test/Docker-Demo.git'             
          }
        }
	stage('Static Code Analysis') {
      steps {
        withSonarQubeEnv(credentialsId: 'adminsonar-token', installationName: 'sonarqube') {
          sh '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn -Dsonar.test.exclusions=**/test/java/servlet/createpage_junit.java -Dsonar.login=admin -Dsonar.password=sonar -Dsonar.tests=. -Dsonar.inclusions=**/test/java/servlet/createpage_junit.java -Dsonar.sources=. sonar:sonar -Dsonar.host.url=http://34.83.65.36:9000'
        }

      }	 
 }
}
