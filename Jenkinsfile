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
                script{
                    def pomFile = readMavenPom file: 'pom.xml'
                    def version = pomFile.version
                    def nexusRepo = version.endsWith ("SNAPSHOT") ? "WiproProject-snapshots" : "WiproProject-release"
                    nexusArtifactUploader artifacts: [[artifactId: 'WiproProject', classifier: '', file: 'target/WiproProject.war', type: 'war']], 
                        credentialsId: 'nexus3', 
                        groupId: 'in.wipro', 
                        nexusUrl: '172.31.47.115:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'WiproProject-snapshots', 
                        version: version
                }
            }
        }
        stage('Tomcat deploy'){
            steps{
                sshagent(['tomcat-dev']) {
                    // copy of war file to tomcat webapps
                    sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@13.233.151.154:/opt/tomcat8/webapps/WiproProject.war"
                    // stop and start tomcat
                    sh "ssh ec2-user@13.233.151.154 /opt/tomcat8/bin/shutdown.sh"
                    sh "ssh ec2-user@13.233.151.154 /opt/tomcat8/bin/startup.sh"
                }
            }
        }
    }
}
