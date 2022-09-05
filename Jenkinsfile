pipeline{
    agent any
    environment {
        PATH = "$PATH:/usr/bin/"
    }

    stages{
       stage('checkout'){
	    checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/fallenmaverick/java-login.git']]])
	}
       stage('Build'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('SonarQube analysis') {
            steps{
	        withSonarQubeEnv('sonarqube-8.9.9') { 
                    sh "mvn sonar:sonar"
            }       
        }
        
        stage('Deploy') {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'deploy', path: '', url: 'http://3.108.194.173:8080')], contextPath: null, war: '**/**.war'
            }
        }
       
     }
}
