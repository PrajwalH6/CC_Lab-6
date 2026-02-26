pipeline {
    agent any

    stages {

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t backend-app backend'
            }
        }

        stage('Run Backend Containers') {
            steps {
                sh '''
                docker rm -f backend1 backend2 nginx-lb || true
                docker network rm lab6-net || true
                docker network create lab6-net || true

                docker run -d --name backend1 --network lab6-net backend-app
                docker run -d --name backend2 --network lab6-net backend-app
                '''
            }
        }

        stage('Run NGINX Load Balancer') {
            steps {
                sh '''
                sleep 3

                docker run -d --name nginx-lb \
                --network lab6-net \
                -p 80:80 \
                nginx

                sleep 2

                docker cp nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf

                docker exec nginx-lb nginx -s reload
                '''
            }
        }
    }
}