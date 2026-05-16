pipeline {
    agent {
        node {
            label 'dockerhost-build-server'
        }
    }

    tools {
        maven 'maven-3.9.6'
    }

    stages {
        stage('Packaging') {
            steps {
                echo 'Packaging..'
                sh 'mvn clean package'
            }
        }

        stage('Copying jar file') {
            steps {
                echo 'Copying jar file..'
                sh 'mv target/*.jar .'
            }
        }

        stage('Cleanup') {
            steps {
                echo 'Cleaning up old Docker resources..'
                sh 'docker rm -f campaign-demo-server || true'
                sh 'docker system prune -a --volumes --force --filter "label=campaign-demo-server"'
            }
        }

        stage('Build image') {
            steps {
                echo 'Building Docker image..'
                sh 'docker build -t paesdevops/campaign-demo:v1 --label campaign-demo-server .'
            }
        }

        stage('Run container') {
            steps {
                echo 'Running Docker container..'
                sh 'docker run -d --name campaign-demo-server --label campaign-demo-server -p 5000:5000 paesdevops/campaign-demo:v1'
            }
        }
    }
}
