pipeline {
    agent any

    stages {
        stage('Git Checkout') {

        }
        stage ('Maven Build') {
            steps {
                sh "mvn clean compile package"
            }
        }
        stage ('Deploy Artifact to Tomcat') {
            steps {
                scp target/  /home/ec2-user/apache-tomcat-11.0.9/webapps
            }
        }
    }
}