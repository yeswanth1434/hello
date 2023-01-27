pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('git checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-creds', url: 'https://github.com/yeswanth1434/hello'
            }
        }
        stage('maven package') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('tomcat deploy') {
            steps {
                sshagent(['tomcat-creds']) {
                    sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.6.93:/opt/tomcat9/webapps"
                    sh "ssh ec2-user@172.31.6.93 /opt/tomcat9/bin/shutdown.sh"
                    sh "ssh ec2-user@172.31.6.93 /opt/tomcat9/bin/startup.sh"
                }
            }
        }
    }
}
