pipeline {
    agent any
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
                    sh 'cd $HOME/workspace/web/Docker-web && docker build -t mvpar/vproweb .'
                    sh 'cd $HOME/workspace/web/Docker-app && docker build -t mvpar/vproapp .'
                    sh 'cd $HOME/workspace/web/Docker-db && docker build -t mvpar/vprodb .'
                }
                
            }
        }
        stage('push') {
            steps {
                script {
                   withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'REGISTRY_PWD', usernameVariable: 'REGISTRY_USER')]) {
                       sh 'docker login -u=$REGISTRY_USER -p=$REGISTRY_PWD'    
                       sh 'docker push mvpar/vproweb'
                       sh 'docker push mvpar/vproapp'
                       sh 'docker push mvpar/vprodb'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh "cd $HOME/workspace/web/compose && docker-compose down && docker-compose up -d"
                }
            }
        }      
    }
}
