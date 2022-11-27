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
    }
}