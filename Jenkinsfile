pipeline {
    agent any

    environment {
    VERSION ="1.${env.BUILD_NUMBER}"
    }

    stages {
        stage('checkout') {
            steps {
                script {
                    git changelog: false, poll: false, url: 'https://github.com/Anji399/visualpath.git'
                }    
            }
        }
        stage('build') {
            steps {
                script {
                    echo "My Image is ${VERSION}"
                    sh 'cd $HOME/workspace/web/Docker-web && docker build -t mvpar/vproweb:${VERSION} .'
                    sh 'cd $HOME/workspace/web/Docker-app && docker build -t mvpar/vproapp:${VERSION} .'
                    sh 'cd $HOME/workspace/web/Docker-db && docker build -t mvpar/vprodb:${VERSION} .'
                }
                
            }
        }
        stage('push') {
            steps {
                script {
                   withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'REGISTRY_PWD', usernameVariable: 'REGISTRY_USER')]) {
                       sh 'docker login -u=$REGISTRY_USER -p=$REGISTRY_PWD'    
                       sh 'docker push mvpar/vproweb:${VERSION}'
                       sh 'docker push mvpar/vproapp:${VERSION}'
                       sh 'docker push mvpar/vprodb:${VERSION}'
                    }
                }
            }
        }
        stage('Deploy') {
            
    }
}
