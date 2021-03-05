pipeline {
    agent { label 'aws-node' }
    triggers {
        pollSCM 'H * * * *'
    }
    stages {
        stage('clone') {
            steps {
                git 'https://github.com/devopsdesk/game-of-life.git'
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
        stage('Deploy') { 
            steps {
                sh 'cp gameoflife-web/target/*.war /home/ec2-user/apache-tomcat-8.5.63/webapps' 
            }
        }    
    }
}
