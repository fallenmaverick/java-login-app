pipeline{
    agent any
    environment{
	    PATH = "/usr/bin:$PATH"
    }
       stages{
       stage('GetCode'){
            steps{
                git 'https://github.com/fallenmaverick/java-login.git'
            }
       }   
	    
       stage ('Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install' 
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml' 
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
                deploy adapters: [tomcat8(credentialsId: 'tomcat-cred', path: '', url: 'http://3.108.194.173:8080/')], contextPath: null, war: '**/*.war'
        }
    }
       
    }
}
