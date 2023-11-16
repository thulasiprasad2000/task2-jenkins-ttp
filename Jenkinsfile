pipeline {
    agent any
    environment {
        YOUR_NAME = credentials("YOUR_NAME")
    }
  
    stages {
        stage('Build') {
            steps {
                sh '''
                echo 'this is test message from Thulasi from jenkinsfile - task2'
                echo 'this is test message from Thulasi from jenkinsfile - task2 second page'
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
                ssh jenkins@thulasi-deploy <<EOF
                export YOUR_NAME=${YOUR_NAME}
                docker network rm task2-net && echo "removed network" || echo "network already removed"
                docker network create task2-net

                docker stop nginx && echo "Stopped nginx" || echo "nginx is not running"
                docker rm nginx && echo "removed nginx" || echo "nginx does not exist"

                docker stop flask-app && echo "Stopped flask-app" || echo "flask-app is not running"
                docker rm flask-app && echo "removed flask-app" || echo "flask-app does not exist"

                docker stop mysql && echo "Stopped mysql" || echo "mysql is not running"
                docker rm mysql && echo "removed mysql" || echo "mysql does not exist"

                docker run -d --name mysql --network task2-net -e MYSQL_ROOT_PASSWORD=PaSSword123 thulasiprasad2000/task2-db
                docker run -d --name flask-app  --network task2-net -e MYSQL_ROOT_PASSWORD=PaSSword123 thulasiprasad2000/task2-app
                docker run -d --name nginx  --network task2-net -p 80:80 thulasiprasad2000/task2-nginx
               
                '''
            }

        }

    }

}

