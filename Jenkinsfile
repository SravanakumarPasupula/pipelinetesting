pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf pipelinetesting'
            sh 'git clone https://github.com/SravanakumarPasupula/pipelinetesting.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /var/lib/jenkins/workspace/pipeline2/pipelinetesting'
            sh 'cp  /var/lib/jenkins/workspace/pipeline2/pipelinetesting/* /var/lib/jenkins/workspace/pipeline2'
            sh 'docker build -t sravanakumar28/pipelinetestdevelop:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push sravanakumar28/pipelinetestdevelop:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.0.0.100:2375 stop devwebapp1 || true'
            sh    'docker -H tcp://10.0.0.100:2375 run --rm -dit --name devwebapp1 --hostname devwebapp1 -p 9000:80 sravanakumar28/pipelinetestdevelop:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.0.0.100:9000'
          }
        }

    }
}
