pipeline {

    agent any

    tools {
            maven 'Maven 3'  // Nom de l'installation Maven configurée

        }

    stages {

        stage('Git Checkout') {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/LinhoDevops/demo-devops.git'

                }
            }
        }
        stage('UNIT testing') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing') {
            steps {
                script {
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }

        //
        stage('Static code analysis'){

                    steps{

                        script{

                            withSonarQubeEnv(credentialsId: 'sonar-api-key') {

                                sh 'mvn clean package sonar:sonar'
                            }
                           }

                        }
                    }
        //

        stage('Nexus repository'){

                        steps{

                            script{

                               nexusArtifactUploader artifacts: [
                               [    artifactId: 'demo-devops-app',
                                    classifier: '',
                                     file: 'target/demo-devops-app-1.0.0.jar',
                                      type: 'jar']],
                                credentialsId: 'nexus-auth',
                                groupId: 'com.linho',
                                nexusUrl: 'localhost:8018',
                                 nexusVersion: 'nexus3',
                                  protocol: 'http',
                                  repository: 'nexus-releases-maven',
                                   version: '1.0.0'
                            }
                        }
                    }

    }
}
