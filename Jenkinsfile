node{
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '7')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    def mavenHome= tool name: "maven3.8.4"
stage('CheckoutCode'){
    git branch: 'development', credentialsId: 'ae5e3e49-4790-43db-b26a-3647438cef04', url: 'https://github.com/mss-devops-jan2022/maven-web-application.git'
}
stage('Build'){
     sh "${mavenHome}/bin/mvn clean package"
}
stage('ExecuteSonarQubeReport'){
     sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('UploadArtifactsintoNexus'){
     sh "${mavenHome}/bin/mvn deploy"
}
stage('DeployApplicationIntoTomcatServer'){
     sshagent(['1f80bc1c-6f03-4525-990b-afb1a0ac3c45']){
     sh  "scp -o StrictHostKeyChecking=no target/maven-web-application.war  ec2-user@13.233.119.38:/opt/apache-tomcat-9.0.56/webapps/"
}
}
}//node close
