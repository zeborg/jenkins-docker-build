node {
  stage 'Checkout to repo'
  checkout scm
  
  stage 'Build and Push Docker image'
  docker.withRegistry('https://ghcr.io', 'dh') {
    def image = docker.build("zeborg/jenkins-pipeline-test:${env.BUILD_ID}")
    image.push()
    image.push('latest')
  }
  
  stage 'Restart existing container with latest image'
  sh '''if [ $( docker ps | grep test-container | wc -l ) -gt 0 ]; then
    docker stop test-container
    docker rm test-container
  fi
  '''
  sh "docker run -d -p 80:80 --mount 'type=volume,src=$PWD,dst=/usr/share/nginx/html' --name test-container ghcr.io/zeborg/jenkins-pipeline-test:latest"
}
