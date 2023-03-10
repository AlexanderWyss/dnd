node {
    stage('Clone repository') {
        checkout scm
    }
    stage('Dependencies Server') {
        sh 'npm ci'
    }
    stage('Build Server') {
        sh 'npm run build --prod'
    }
    stage('Dependencies Client') {
        dir('client') {
            sh 'npm ci'
        }
    }
    stage('Build Client') {
        dir('client') {
            sh 'npm run build:prod --prod'
        }
    }
    stage('Build Docker image') {
        docker.build("alexanderwyss/dnd")
    }
    stage('Deploy') {
        sh 'docker stop dnd || true && docker rm -f dnd || true'
        sh 'docker run -d --expose 8080 --restart unless-stopped --name dnd -e NODE_ENV=production -e PORT=8080 -e VIRTUAL_HOST=dnd.wyss.tech -e VIRTUAL_PORT=8080 -e LETSENCRYPT_HOST=dnd.wyss.tech alexanderwyss/dnd:latest'
    }
}
