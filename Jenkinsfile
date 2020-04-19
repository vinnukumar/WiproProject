pipeline{
    agent any
    tools {
        maven 'maven3'
    }
    stages{
        stage('Maven Build/Package'){
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
        stage('Tomcat Deploy'){
            steps{
                def userHost = "ec2-user@172.31.1.251"
                def tomcatBin = "ec2-user@ec2-user@172.31.1.251 /opt/tomcat8/bin"
                sshagent(['tomcat-dev']) {
                    // copy war file to tomcat webapps
                    sh "scp -o StrictHostKeyChecking=no target/*.war ${userHost} : /opt/tomcat8/webapps/WiproProject.war"
                    // stop and start tomcat
                    sh "ssh ${tomcatBin}/shutdown.sh"
                    sh "ssh ${tomcatBin}/startup.sh"
                }
            }
        }
    }
}
