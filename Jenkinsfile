pipeline {
    agent {
        node {
	    label 'jenkins-slave'
        }
    }

    stages{
       def mvnHome = tool 'Maven3'
       stage('checkout'){
	    checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/fallenmaverick/java-login.git']]])
	}
       stage('Build'){
            steps{
                sh '"$(usr/bin/mvn clean install -f java-login/pom.xml'
            }
        }
        stage ('Code Quality scan')  {
            withSonarQubeEnv('SonarQube') {
            sh "${mvnHome}/bin/mvn sonar:sonar -f java-login/pom.xml"
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
