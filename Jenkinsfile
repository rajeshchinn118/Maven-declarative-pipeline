pipeline {
    agent any
    tools {
    maven 'maven3.8.6'
    }
    triggers {
        pollSCM 'H * * * *'
    }
    stages {
        stage('clone') {
            steps {
                git 'https://github.com/devopsdesk/java-web-app-docker.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('archive artifacts') { 
            steps {
                archiveArtifacts artifacts: '**/target/*.war', followSymlinks: false 
            }
            
        }
        stage('deploy') { 
            steps {
                deploy adapters: [tomcat9(credentialsId: 'admin', path: '', url: 'http://13.126.44.102:8080/')], contextPath: 'java-web-app', war: '**/*.war'
            }            
        }
          
    }
}
