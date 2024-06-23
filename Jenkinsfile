pipeline {
    agent any
    environment{
        PATH = "/opt/maven3/bin:$PATH"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/yelurusrinu/HelloWorld.git'
            }
        }
        stage('Build') {
            steps {
                 dir('HelloWorld') {
                sh 'mvn clean package'
                sh "mv target/*.war target/HelloWorld0.0.1-SNAPSHOT.war"
                 }
            }
        }
        stage('Deploy') {
            steps {
                sshagent (['ubuntu']) {
                    dir('HelloWorld') {
                    sh 'ls -l target'
                    
                    // SCP the WAR file to remote server
                    sh 'scp -o StrictHostKeyChecking=no target/HelloWorld 0.0.1-SNAPSHOT.war ubuntu@172.31.35.81:/home/ubuntu/apache-tomcat-9.0.90/webapps/'
                    
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.35.81 sudo chown -R ubuntu:ubuntu /home/ubuntu/apache-tomcat-9.0.90/webapps/HelloWorld'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.35.81 sudo chmod -R 755 /home/ubuntu/apache-tomcat-9.0.90/webapps/HelloWorld'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.35.81 sudo /home/ubuntu/apache-tomcat-9.0.90/bin/shutdown.sh'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.35.81 sudo /home/ubuntu/apache-tomcat-9.0.90/bin/startup.sh'
                }
                }
            }
        }
    }
}
