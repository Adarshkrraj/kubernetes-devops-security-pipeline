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

        stage('Sonarqube - SAST') {
            tools   {
                maven 'Maven-install'
            }
                steps {

                    withSonarQubeEnv('sonarqube') { //server added
                        sh "mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=Numeric-application -Dsonar.projectName='Numeric-application'"
                    }
                }
            }
        stage('docker build and push'){
            steps {
                withDockerRegistry([credentialsId:"Docker-hub", url:""]){
                    sh 'printenv'
                    sh "docker build -t adarshkumar410/numeric-application:${env.GIT_COMMIT} ."
                    // Uncomment below when you restore your withDockerRegistry block
                    sh "docker push adarshkumar410/numeric-application:${env.GIT_COMMIT}"
                }

            }
        }
        stage('kubernetes deployment - dev'){
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']){
                sh "sed -i 's#replace#adarshkumar410/numeric-application:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
                sh "kubectl apply -f k8s_deployment_service.yaml"
                }
            }
        }
    }
}