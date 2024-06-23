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
                dir('HelloWorld'){
                sh 'mvn clean package'
                }
            }
        }
        stage('Deploy') {
            steps {
                sshagent (['ubuntu']) {
                    sh '''
                        scp -o StrictHostKeyChecking=no target/HelloWorld-0.0.1-SNAPSHOT.war ubuntu@3.6.40.50:/home/ubuntu/
                        ssh -o StrictHostKeyChecking=no ubuntu@3.6.40.50 'sudo cp /home/ubuntu/HelloWorld-0.0.1-SNAPSHOT.war /var/lib/apache-tomcat-9.0.90/webapps/helloworld.war && sudo systemctl restart apache-tomcat-9.0.90'
                    '''
                }
            }
        }
    }
}
