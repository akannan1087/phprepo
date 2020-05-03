node {
  
  def image
  
     stage ('checkout') {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '9ffd4ee4-3647-4a7d-a357-5e8746463282', url: 'https://bitbucket.org/ananthkannan/phprepo/']]])       
        }
    
    stage ('Docker Build') {
         // Build and push image with Jenkins' docker-plugin
        withDockerServer([uri: "tcp://localhost:4243"]) {

            withDockerRegistry([credentialsId: "1ae9f8ea-826b-4d2c-9a11-344647f3cdf3", url: "https://index.docker.io/v1/"]) {
            image = docker.build("ananthkannan/myphpapp")
            image.push()
            }
        }
    }
    
       stage('docker stop container') {
            sh 'docker ps -f name=myPhpContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=myPhpContainer -q | xargs -r docker container rm'

       }

    stage ('Docker run') {
        image.run("-p 8086:80 --rm --name myPhpContainer")

    }
}