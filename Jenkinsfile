node{
    
 def mavenHome = tool name: "maven 3.8.2" 
      
  echo "GitHub BranhName ${env.BRANCH_NAME}"
  echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"

 stage('CheckoutCode')
 {
 git branch: 'development', credentialsId: 'b17ed7ea-f3ff-47d5-92a0-b99a0115838a', url: 'https://github.com/SATYA719/maven-web-application.git'
 }


 stage('Build')
 {
  sh "${mavenHome}/bin/mvn clean package"
 }
 
 stage('SonarQubeReport'){
    sh "${mavenHome}/bin/mvn clean sonar:sonar" 
 }
 
  stage('UploadArtifactIntoNexus'){
    sh "${mavenHome}/bin/mvn clean deploy" 
 }
 
 stage('DeployAppIntoTomcatserver')
 {
  sshagent(['d554353e-f1f8-41ef-8f7d-b206782e1779']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.92.119:/opt/apache-tomcat-9.0.59/webapps/"
}
 }
 stage('SendEmailNotification'){
     emailext body: '''Build is over !!

     Regards,
     mithuntechnologies''', subject: 'Build Over...!', to: 'formula1satya@gmail.com'
 }
 
}
