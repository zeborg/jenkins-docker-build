node {
  checkout scm
  
  docker.withRegistry('https://ghcr.io', 'dh') {
    def image = docker.build("zeborg/jenkins-pipeline-test:${env.BUILD_ID}")
    image.push()
    image.push('latest')
  }
}
