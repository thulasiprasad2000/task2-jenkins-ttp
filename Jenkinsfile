pipeline {
    agent any
    environment {
        YOUR_NAME = credentials("YOUR_NAME")
        MYSQL_ROOT_PASSWORD = credentials("MYSQL_ROOT_PASSWORD")
    }
  
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t thulasiprasad2000/task2-db db
                docker build -t thulasiprasad2000/task2-app flask-app
                docker build -t thulasiprasad2000/task2-nginx nginx
                '''
            }

        }
        stage('Push') {
            steps {
                sh '''
                docker push thulasiprasad2000/task2-app
                docker push thulasiprasad2000/task2-db
                docker push thulasiprasad2000/task2-nginx
                '''
            }

        }
        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f .
                sleep 60
                kubectl get services
                '''
            }

        }

    }

}

