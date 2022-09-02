pipeline{
    agent any
    environment {
        PATH = "$PATH:/usr/share/maven/bin"
    }
    stages{
       stage('GetCode'){
            steps{
                git 'https://github.com/fallenmaverick/java-login.git'
            }
         }        
       stage('Build'){
            steps{
                sh 'mvn clean install'
            }
         }
        stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('sonarqube-8.9.2') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/sonar-scanner"
        sh "mvn sonar:sonar"
    }
        }
        }
	stage('Deploy') {
	  steps {   
            deploy adapters: [tomcat8(credentialsId: 'deploy', path: '', url: 'http://3.110.86.225:8080')], contextPath: null, war: '**/**.war'
      }
    }
       
    }
}
