pipeline {
    agent any
    tools {
    maven 'Maven3'
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
                deploy adapters: [tomcat9(credentialsId: 'admin', path: '', url: 'http://3.110.148.3:8081/')], contextPath: 'java-web-app', war: '**/*.war'
            }            
        }
          
    }
}
