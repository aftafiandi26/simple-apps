pipeline {
    agent { label 'dev1-aftafiandi' }
    
    tools {nodejs "NodeJS-18.16.0"}

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
                npm run test:coverage'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''
                sonar-scanner \
                -Dsonar.projectKey=simple-apps-2 \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.11.113:9000 \
                -Dsonar.login=sqp_0c3bd05303d87bb64cf752718fe494cbf13d0945'''
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
                  docker tag simple-apps-pipeline hattpri/simple-apps-pipeline
                  docker push hattpri/simple-apps-pipeline
                  docker image prune -a -f
                  '''
              }
          }
    }
}
