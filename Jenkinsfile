pipeline {
    agent any
	
    stages{
        
       stage('GetCode'){
            steps{
            git 'https://github.com/fallenmaverick/java-login.git'
            }
        } 
        stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0.0';
            steps{
                withSonarQubeEnv('SonarQube') { 
// If you have configured more than one global server connection, you can specify its name
//           sh "${scannerHome}/bin/sonar-scanner"
                sh "mvn sonar:sonar"
                }
            }
            } 
       stage('Build'){
            steps{
                sh 'mvn install -f java-login/pom.xml'
            }
        }
        
        stage('Test') {
            steps{
                sh 'mvn test'
            }
            post {
                success {
                junit '**/target/surefire-reports/TEST-*.xml'
                }
            }
        }
		
        stage('Deploy') {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'tomcat-cred', path: '', url: 'http://13.232.188.67:8080')], contextPath: null, war: '**/*.war'
            }
        }
       
    }
}
