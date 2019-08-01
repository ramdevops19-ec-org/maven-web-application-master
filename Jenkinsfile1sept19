node() 
{
    
    def MavenHome = tool name :'Maven3.61', type :'maven'
    
    properties([
       buildDiscarder(logRotator(numToKeepStr: '3')),
       pipelineTriggers([
           pollSCM('* * * * *')
       ])
   ])
        stage('Checkout'){
        git branch: 'development', credentialsId: '70567c72-5c1c-4292-99ff-fbd59ccb421e', url: 'https://github.com/ramdevops19-ec-org/maven-web-application-master.git'
            }
            stage('Build')
            {
                sh "${MavenHome}/bin/mvn clean package"
            }
            stage('ExecuteSonarQubeReport')
            {
              sh "${MavenHome}/bin/mvn sonar:sonar"  
            }
            stage('UploadArtifact')
            {
              sh "${MavenHome}/bin/mvn deploy"  
            }
            
             stage('DeployApplication')
            {
              sshagent(['d534ba9f-a2e5-4764-96d8-1aef134345af']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@18.130.86.182:/opt/apache-tomcat-9.0.21/webapps/maven-web-application.war"
}  
            }
}
