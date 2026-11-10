pipeline {
    agent any

    stages {
        stage('Generate site') {
            steps {
                sh 'mkdir -p site'
                writeFile file: 'site/index.html',
                    text: '<h1>Welcome to Karunya University</h1>'
            }
        }

        stage('Publish site') {
            steps {
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'site',
                    reportFiles: 'index.html',
                    reportName: 'Karunya Website'
                ])
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'site/**'
        }
    }
}


docker-compose.yml


services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock

volumes:
  jenkins_home:



