pipeline{
    agent any
    tools {
        maven 'maven3'
    }

    stages{
        stage('Git checkout'){
            steps{
                git credentialsId: 'github', 
                url: 'https://github.com/vinnukumar/WiproProject'
            }
        }
        stage('Maven Deploy/Package'){
            steps{
                sh 'mvn clean package'
            }
        }
    }
}
