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