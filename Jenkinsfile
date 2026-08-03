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
		stages('docker build and push'){
		steps{
		sh 'printenv'
		sh 'docker build -t adarshkumar410/numeric-application:""$GIT_COMIT"".'
		sh 'docker push adarshkumar410/numeric-application:""$GIT_COMMIT""'
		}
		}
    }
}