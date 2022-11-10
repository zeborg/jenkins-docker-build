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
  sh "docker stop test-container"
  sh "docker rm test-container"
  sh "docker run -d -p 80:80 --name test-container ghcr.io/zeborg/jenkins-pipeline-test:latest"
}
