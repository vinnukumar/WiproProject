pipeline{
    agent any
    tools {
        maven 'maven3'
    }

    stages{
        stage('Maven Deploy/Package'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Nexus Deploy'){
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'WiproProject', classifier: '', file: 'target/WiproProject.war', type: 'war']], 
                credentialsId: 'nexus3', 
                groupId: 'in.wipro', 
                nexusUrl: '172.31.47.115:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'WiproProject-snapshots', 
                version: '1.0-SNAPSHOT'
            }
        }
    }
}
