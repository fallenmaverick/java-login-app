
pipeline {
    agent {
	    node {
	    label 'jenkins-slave'
        }
    }

    stages {
//  Use this code for inline pipeline script option
        stage('Code checkout') {
            steps {
        //download code from github
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/fallenmaverick/jenkins-cicd-java-maven-demo.git']]])
        }

    }
	    stage('SonarQube analysis') {
//  Def scannerHome = tool 'SonarScanner 4.0.0';
            steps{
                withSonarQubeEnv('SonarQube') { 
//  If you have configured more than one global server connection, you can specify its name
//              sh "${scannerHome}/bin/sonar-scanner"
                sh "mvn sonar:sonar"
                }
            }
        }
	
        stage('Build') {
            steps {
        // Run the maven build
            sh '"mvn" -Dmaven.test.failure.ignore clean install'
            }
	    }
	
	    stage('Test') {
	        steps {
		    sh '"mvn" -Dmaven.test.failure.ignore test'
		    }
			post {
			    always {
                archiveArtifacts artifacts: 'build/libs/**/*.jar', fingerprint: true
                junit '**/reports/TEST-*.xml'
                }
            }
	    }
	  
        stage('Deploy') {
            steps {
//    deploy war on tomcat server
            deploy adapters: [tomcat8(credentialsId: 'tomcat-cred', path: '', url: 'http://15.207.86.173:8080/')], contextPath: null, war: '**/*.war'
		    }
        }
    }
}
