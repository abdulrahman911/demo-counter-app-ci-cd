pipeline {
    agent any 
    stages {
        
        stage('Clonning Git repository'){
            steps{
                git branch: 'main', url: 'https://github.com/abdulrahman911/demo-counter-app-ci-cd.git'
            }
        }

        stage('UNIT Testing'){
            steps{
               sh 'mvn test'    
            }
            
        }

        stage('Integration Testing'){
            steps{
                sh 'mvn verify -DskipUnitTests'
            }
        }

        stage('Maven build'){
            steps{
                sh 'mvn clean install'
            }
        }    

        stage('Static code Analysis'){
            steps{
                script{
                withSonarQubeEnv(credentialsId: 'sonar-api') {
                 sh 'mvn clean package sonar:sonar'

                }
             }
            }
        }

        stage('Quality gate status'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                }
            }
        }

        stage('Upload war file to nexus'){
            steps{
                script{

                    def readPomVersion = readMavenPom file: 'pom.xml'
                    def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "shopping-app-snapshot" : "shopping-app-club"
                    nexusArtifactUploader artifacts: [
                        [artifactId: 'springboot', 
                        classifier: '', file: 'target/Uber.jar', 
                        type: 'jar']
                        ], 
                        credentialsId: 'nexus-auth', groupId: 'com.example', 
                        nexusUrl: '65.0.178.225:8081', nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        // repository: 'shopping-app-club', 
                        repository: nexusRepo,
                        version: "${readPomVersion.version}"
                }
            }
        }

        stage('Build docker images'){
            steps{
                script{
                    sh 'docker image build -t rahman777/$JOB_NAME:v1.$BUILD_ID .'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID rahman777/JOB_NAME:latest'
                   //sh 'docker image tag $JOB_NAME:v1.$BUILD_ID rahman777/JOB_NAME:latest'
                }
            }
        }
           
    }
}