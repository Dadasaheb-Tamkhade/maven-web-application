node{
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
    def mavenHome = tool name: "maven3.8.4" 

//getting code from github
stage('checkoutcode'){
git branch: 'development', credentialsId: '560a072e-3307-42de-9a7a-63a81d9d5eb8', url: 'https://github.com/Dadasaheb-Tamkhade/maven-web-application.git'
    }

//using maven doing build
stage('build'){
sh "${mavenHome}/bin/mvn clean package"
}

  //execute the sonarqube report      
stage('Executed the sonarqube report'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

//artifact upload to nexus
stage('upload artifact in to nexus'){
sh "${mavenHome}/bin/mvn deploy"
}

//deploy to tomcat server 
stage('deploy app to tomcat server'){
sshagent(['c594b211-ca79-447a-a535-a56992e74105']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.99.128:/opt/apache-tomcat-9.0.58/webapps/"
}
}

}
