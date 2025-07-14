pipeline {
    agent any
    
    tools {
        maven 'maven s/w'
    }
    
    env {
        WAR_NAME = 'hello-world-0.0.1-SNAPSHOT.war'
        WAR_PATH = "target/${WAR_NAME}"
        TOMCAT_WEBAPPS = '/home/ec2-user/apache-tomcat-11.0.9/webapps/'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ashwiniboddu/Hello-World.git'
            }
        }
        stage ('Maven Build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage ('Deploy Artifact to Tomcat') {
            steps {
                sh '''
                cp ${WAR_PATH} ${TOMCAT_WEBAPPS}/hello-wold.war
                '''
            }
        }
    }
}