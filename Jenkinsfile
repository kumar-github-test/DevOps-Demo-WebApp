node {
  
    // Get Artifactory server instance, defined in the Artifactory Plugin administration page
    
    //	def server = Artifactory.server "Artifcatory1"
    
    // Create an Artifactory Maven instance.
    
    	def rtMaven = Artifactory.newMavenBuild()
    	def buildInfo 
	
    // Tool name from Jenkins configuration
    
	rtMaven.tool = "Maven"
	
    // Set Artifactory repositories for dependencies resolution and artifacts deployment.
    
   //     rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server: server
   //     rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
//  
//	slackSend channel: 'alerts', message: "Pipeline Started ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)", teamDomain: 'devopsbc', tokenCredentialId: 'slack'
//	    
    stage('Clone source') {
        git url: 'https://github.com/kumar-github-test/DevOps-Demo-WebApp/'
    }
//    

//    
    stage('Static Code Analysis') {
        withSonarQubeEnv(credentialsId: 'adminsonar-token', installationName: 'sonarqube') { // You can override the credential to be used
       	//sh 'mvn clean package sonar:sonar -Dsonar.host.url=http://52.152.224.93// -Dsonar.sources=. -Dsonar.tests=. -Dsonar.test.inclusions=**/test/java/servlet/createpage_junit.java -Dsonar.exclusions=**/test/java/servlet/createpage_junit.java'
        
	sh '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn -Dsonar.test.exclusions=**/test/java/servlet/createpage_junit.java -Dsonar.login=admin -Dsonar.password=sonar -Dsonar.tests=. -Dsonar.inclusions=**/test/java/servlet/createpage_junit.java -Dsonar.sources=. sonar:sonar -Dsonar.host.url=http://34.83.29.55:9000'	
	}
    				}	    
    stage('Build Web App') {
        buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
    }
//
	jiraSendBuildInfo branch: 'DCS-1', site: 'devopss4.atlassian.net'
    stage('Deploy to Test') {
	deploy adapters: [tomcat8(credentialsId: 'tomcat-1', path: '', url: 'http://52.255.157.89:8080/')], contextPath: '/QAWebapp', war: '**/*.war'
	//jiraSendDeploymentInfo environmentId: 'Test', environmentName: 'QA test', environmentType: 'testing', serviceIds: ['http://52.255.157.89:8080/QAWebapp/'], site: 'devopsbc.atlassian.net', state: 'successful'
    }
//
    stage('Store  Artifacts') {
        server.publishBuildInfo buildInfo
    }
	
      stage('UI Test') {
        buildInfo = rtMaven.run pom: 'functionaltest/pom.xml', goals: 'test'
	publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '\\functionaltest\\target\\surefire-reports', reportFiles: 'index.html', reportName: 'UI Test Report', reportTitles: ''])
    }
     
       stage('Performance Test') {
    	echo 'Running BlazeMeterTest' 
    //blazeMeterTest credentialsId: '917117ed-d257-41d2-bb35-47dea13f959c', testId: '9014498.taurus', workspaceId: '756635'
    }
        stage('Deploy to Prod') {
	      deploy adapters: [tomcat8(credentialsId: 'tomcat-1', path: '', url: 'http://13.68.144.119:8080/')], contextPath: '/ProdWebapp', onFailure: false, war: '**/*.war'
	     //jiraSendDeploymentInfo environmentId: 'Staging', environmentName: 'Staging', environmentType: 'staging', serviceIds: ['http://13.68.144.119:8080/ProdWebapp'], site: 'devopsbc.atlassian.net', state: 'successful'
	     //jiraSendDeploymentInfo environmentId: 'Prod', environmentName: 'prod', environmentType: 'production', serviceIds: ['http://13.68.144.119:8080/ProdWebapp'], site: 'devopsbc.atlassian.net', state: 'successful'
         }
	
        stage('Sanity Test') {
        buildInfo = rtMaven.run pom: 'Acceptancetest/pom.xml', goals: 'test'
	publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '\\Acceptancetest\\target\\surefire-reports', reportFiles: 'index.html', reportName: 'Sanity Test Report', reportTitles: ''])
    			  }
	jiraSendDeploymentInfo enableGating: true, environmentId: '', environmentName: '', environmentType: 'production', issueKeys: ['TDB-1'], serviceIds: [''], site: 'fresco3.atlassian.net', state: 'successful'
	slackSend channel: 'alerts', message: "Pipeline Completed ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)", teamDomain: 'devopsbc', tokenCredentialId: 'slack'
}
