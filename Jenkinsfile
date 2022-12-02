def pipelineContext = [:]
node {

   def registryProjet='registry.gitlab.com/wilgates1/jenkins-build-docker'
   def IMAGE="${registryProjet}:version-${env.BUILD_ID}"

    stage('Clone') {
          git credentialsID: 'gitlab', url:'https://registry.gitlab.com/wilgates1/jenkins-build-docker.git'
    }

    def img = stage('Build') {
          docker.build("$IMAGE",  '.')
    }

    stage('Run') {
          img.withRun("--name run-$BUILD_ID -p 8090:80") { c ->
            sh 'docker ps'
	    sh 'curl localhost:8090'
          }
    }

    stage('Push') {
          docker.withRegistry('https://registry.gitlab.com', 'gitlab') {
              img.push 'latest'
              img.push()
          }
    }

}



