pipeline {
        agent any
        tools
    {
       maven "Maven3.6.3"
       jdk 'JDK'
    }
        
	stages {
		
      //    stage("build & SonarQube analysis") {
       //       steps {
       //       withSonarQubeEnv(credentialsId: 'adminsonar-token', installationName: 'sonarqube') 
	    // { 
    //		sh "/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven3.6.3/bin/mvn -Dsonar.test.exclusions=**/test/java/servlet/createpage_junit.java -Dsonar.login=admin -Dsonar.password=sonar -Dsonar.tests=. -Dsonar.inclusions=**/test/java/servlet/createpage_junit.java -Dsonar.sources=. sonar:sonar -Dsonar.host.url=http://34.83.65.36:9000"
     //         }
      //      }
       //   }
	
	
           // Building web app using Maven build	
           stage("Build Web App") {
                   steps {
                           sh 'mvn -Dmaven.test.failure.ignore=true install'
                          }  
           }
		
         stage('Deploy to Test') {
                 steps {    
	deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://35.202.157.51:8080/')], contextPath: '/QAWebapp', war: '**/*.war'
                         }       
                 }           
                
        }
      }

