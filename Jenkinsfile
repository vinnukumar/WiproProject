pipeline{
    agent any
    tools {
        maven 'maven3'
    }
    stages{
        stage('Git Checkout'){
            steps{
                git credentialsId: 'github', 
                url: 'https://github.com/vinnukumar/WiproProject'
            }
        }
        stage('Maven Build/Package'){
            steps{
                sh 'mvn clean package'
            }
        }
    }
}
