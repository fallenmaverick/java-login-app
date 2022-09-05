
pipeline {
  agent {
    node {
      label 'jenkins-slave'
    }
  }

  stages {
    //Use this code for inline pipeline script option
    stage('Code checkout') {
      steps {
        //download code from github
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/fallenmaverick/jenkins-cicd-java-maven-demo.git']]])
      }

    }
	stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0.0';
       steps{
           withSonarQubeEnv('SonarQube') { 
// If you have configured more than one global server connection, you can specify its name
//         sh "${scannerHome}/bin/sonar-scanner"
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
     stage('Test'){
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
        //deploy war on tomcat server
       deploy adapters: [tomcat8(credentialsId: 'tomcat-cred', path: '', url: 'http://13.232.188.67:8080/')], contextPath: null, war: '**/*.war'

      }
    }
  }
}
