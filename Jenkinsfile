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
                sh 'mvn clean package'
                sh "mv target/*.war target/Helloworld.war"
            }
        }
        stage('Deploy') {
            steps {
                sshagent (['ubuntu']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no target/Helloworld.war  ubuntu@172.31.35.81:/home/ubuntu/apache-tomcat-9.0.90/webapps/
                    ssh ec2-user@172.31.35.81 /home/ubuntu/apache-tomcat-9.0.90/bin/shutdown.sh
                    ssh ec2-user@172.31.35.81 /home/ubuntu/apache-tomcat-9.0.90/bin/startup.sh
                    '''
                }
            }
        }
    }
}
