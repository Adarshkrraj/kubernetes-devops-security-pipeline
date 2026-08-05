pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar'
            }
        }

//        stage('Unit Test') {
//                    steps {
//                     sh "mvn test"
//                   }
//               }
        stage('Unit Tests - JUnit and Jacoco'){
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                    jacoco execPattern: 'target/jacoco.exec'
                }
		}
		}
//	}
//}
 		stage('docker build and push'){
                     steps{
//                      withDockerRegistry([credentialsId:"Docker-hub", url:""]){
                        sh 'printenv'
//                         // Changed external quotes to double quotes, removed unnecessary inner quotes
                        sh "docker build -t adarshkumar410/numeric-application:${GIT_COMMIT} ."
//                         sh "docker push adarshkumar410/numeric-application:${GIT_COMMIT}"
//                     }
//                 }
//                 }
     }
 }