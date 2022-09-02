pipeline{
    agent any
    environment {
        PATH = "$PATH:/usr/share/maven/bin"
    }
    stages{
       stage('GetCode'){
            steps{
                git 'https://github.com/fallenmaverick/java-login-app.git'
            }
         }
   stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('sonarqube-8.9.9') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/SonarQube"
        sh "mvn sonar:sonar"
    }    
	}
	}
       stage('Build'){
            steps{
                sh 'mvn clean package'
            }
         }
	stage('Test'){
		steps{
                sh 'mvn test'
                 }
            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
            }
         }
        
        
	stage('Deploy') {
      steps {   
         deploy adapters: [tomcat8(credentialsId: 'tomcat-cred', path: '', url: 'http://3.110.86.225:8080/')], contextPath: null, war: '**/*.war'
      }
    }
       
    }
}
