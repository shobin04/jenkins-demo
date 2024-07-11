pipeline {
    agent any
    
    stages {
        stage ('SCM checkout') {
            steps {
              git branch: 'main', 
                    url: 'https://github.com/shobin04/jenkins-demo.git'
            }
        }   
        stage('SonarQube') {
          environment {
            SONAR_RUNNER_HOME = tool 'SonarQube'
            PROJECT_NAME = "sample"
          }
          steps {
            withSonarQubeEnv('SonarQube') {
                sh '''cd /var/lib/jenkins/workspace/Demo-Project/java-hello-world-with-maven-master
                mvn clean verify sonar:sonar \
               -Dsonar.projectKey=sample \
               -Dsonar.projectName='sample' \
               -Dsonar.host.url=http://54.227.43.162:9000 \
               -Dsonar.token=sqp_f864753e09846383d7dffce1302d4376944d454d
              '''
             }
          }
       }
         stage ('build') {
            steps {
               sh""" cd /var/lib/jenkins/workspace/Demo-Project/java-hello-world-with-maven-master
               mvn package
               """
            }
        }  
        stage ('deploy') {
            steps {
                 sh"""sudo cp /var/lib/jenkins/workspace/Demo-Project/java-hello-world-with-maven-master/target/jb-hello-world-maven-0.2.0.jar /opt/tomcat/webapps
                 sudo systemctl restart tomcat
                 """
            }
        }
    }
}
