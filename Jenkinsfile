pipeline {
    agent any

    environment {
    VERSION ="1.${env.BUILD_NUMBER}"
    }

    stages {
        stage('checkout') {
            steps {
                script {
                    git changelog: false, poll: false, url: 'https://github.com/Anji399/vp-docker.git'
                }
                
            }
        }
        stage('build') {
            steps {
                script {
                    echo "My Image is ${VERSION}"
                    sh 'cd /var/lib/jenkins/workspace/web/Docker-web'
                    sh 'docker build -t mvpar/vproweb:${VERSION} .'
                }
                
            }
        }
        stage('push') {
            steps {
                script {
                   withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'REGISTRY_PWD', usernameVariable: 'REGISTRY_USER')]) {
                       sh 'docker login -u=$REGISTRY_USER -p=$REGISTRY_PWD'    
                       sh 'docker push mvpar/vproweb:${VERSION}'
                    }
                }
            }
        }
    }
}
