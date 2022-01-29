pipeline {
    agent { label 'rhel-node' }
    tools {
    maven 'maven3.8.4'
    }
    triggers {
        pollSCM 'H * * * *'
    }
    stages {
        stage('clone') {
            steps {
                git 'https://github.com/daticahealth/java-tomcat-maven-example.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') { 
            steps {
                sh 'mvn test' 
            }
            post {
                always {
                    junit 'gameoflife-core/target/surefire-reports/*.xml'
                }
            }
        }
          
    }
}
