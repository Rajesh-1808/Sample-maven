pipeline {
    agent any

    tools {
        java "JDK17"
        maven "M3"
    }

    environment {
        APP_NODE_IP   = "34.245.23.133"
        APP_NODE_USER = "ubuntu"
        APP_NODE_KEY  = "/var/lib/jenkins/.ssh/appnode.pem"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Rajesh-1808/Sample-maven.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh """
                  scp -o StrictHostKeyChecking=no -i ${APP_NODE_KEY} target/*.war ${APP_NODE_USER}@${APP_NODE_IP}:/opt/tomcat/webapps/

                  ssh -o StrictHostKeyChecking=no -i ${APP_NODE_KEY} ${APP_NODE_USER}@${APP_NODE_IP} "sudo systemctl restart tomcat"
                """
            }
        }
    }

    post {
        success {
            echo '✅ Build, Test, and Deployment Successful! Tomcat restarted.'
        }
        failure {
            echo '❌ Something went wrong.'
        }
    }
}
