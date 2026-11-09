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


#nginx (nginx-pod.yml)


apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80

