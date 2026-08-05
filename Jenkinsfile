pipeline {
    agent any

    stages {
        stage('Build Artifact') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
            }
        }

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

        stage('docker build and push'){
            steps {
                sh 'printenv'
                sh "docker build -t adarshkumar410/numeric-application:${env.GIT_COMMIT} ."
                // Uncomment below when you restore your withDockerRegistry block
                sh "docker push adarshkumar410/numeric-application:${env.GIT_COMMIT}"
            }
        }
    }
}