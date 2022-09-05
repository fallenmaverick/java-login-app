pipeline{
    agent any
	environment{
		maven "Maven3"
    }
    stages{
       stage('GetCode'){
            steps{
                git 'https://github.com/fallenmaverick/java-login.git'
            }
         }        
       stage('Build'){
            steps{
                sh "mvn clean install"
            }
        }
        stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
            steps{
		 withSonarQubeEnv('sonarqube-8.9.2') { 
		     sh "mvn sonar:sonar"
    }
        }
        }
	stage('Deploy') {
	    steps {   
                deploy adapters: [tomcat8(credentialsId: 'deploy', path: '', url: 'http://3.108.194.173:8080')], contextPath: null, war: '**/**.war'
        }
    }
       
    }
}
