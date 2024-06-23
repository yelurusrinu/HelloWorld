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
                sh "mv target/*.war target/Helloworld.war"
                 }
            }
        }
        stage('Deploy') {
            steps {
                sshagent (['ubuntu']) {
                    dir('HelloWorld') {
                    sh 'ls -l target'
                    
                    // SCP the WAR file to remote server
                    sh 'scp -o StrictHostKeyChecking=no target/HelloWorld.war ubuntu@172.31.35.81:/home/ubuntu/apache-tomcat-9.0.90/webapps/'
                    
                    // Shutdown and restart Tomcat on the remote server
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.35.81 /home/ubuntu/apache-tomcat-9.0.90/bin/shutdown.sh'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.35.81 /home/ubuntu/apache-tomcat-9.0.90/bin/startup.sh'
                }
                }
            }
        }
    }
}
