pipeline {
    agent { label 'dev1-aftafiandi' }
    
    tools {nodejs "NodeJS-18.16.0"}

    environment {
        NAMEAPP = 'simple-apps-pipeline-apps'
        SONARHOST = 'http://172.23.11.113:9000'
        TOKENSONAR = 'sqp_0c3bd05303d87bb64cf752718fe494cbf13d0945'  
        VERSION = 'V1.0.0'
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                npm install'''
            }
        }
        stage('Testing') {
            steps {
                sh '''
                npm test
                '''
            }
        }
        stage('Code Review') {
            steps {
                sh '''
                sonar-scanner \
                -Dsonar.projectKey=simple-apps-2 \
                -Dsonar.sources=. \
                -Dsonar.host.url=${SONARHOST} \
                -Dsonar.login=${TOKENSONAR}'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }
        stage('Deploy Registery Image') {
              steps {
                  sh '''
                  docker tag ${NAMEAPP} hattpri/${NAMEAPP}:${VERSION}
                  docker push hattpri/${NAMEAPP}:${VERSION}
                  docker image prune -a -f
                  '''
              }
          }
    }
}
