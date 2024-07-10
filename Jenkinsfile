pipeline {
    agent any
    
    stages {
        stage ('SCM checkout') {
           steps {
               git 'https://github.com/shobin04/jenkins.git'
            }
        }   
    stage('SonarCloud') {
          environment {
            SONAR_RUNNER_HOME = tool 'SonarQube Scanner'
            PROJECT_NAME = "jenkins"
          }
          steps {
            withSonarQubeEnv('SonarCloudOne') {
                sh '''$SONAR_RUNNER_HOME/opt/sonar-scanner \
                -Dsonar.java.binaries=build/classes/java/ \
                -Dsonar.projectKey=$PROJECT_NAME \
                -Dsonar.sources=.'''
             }
          }
       }
        stage ('build') {
            steps {
               sh""" cd /var/lib/jenkins/workspace/Demo-Project/java-maven-app-master/
               mvn package
               """
            }
        }  
        stage ('deploy') {
            steps {
                 sh""" sudo cp /var/lib/jenkins/workspace/Demo-Project/java-maven-app-master/target/my-app-1.0-SNAPSHOT.war /opt/tomcat/webapps/
                 sudo systemctl restart tomcat
                 """
            }
        }   
    }
}
