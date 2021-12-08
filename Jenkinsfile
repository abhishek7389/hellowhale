pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/brijgopal/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("brijgopal/jenk1:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
           sh "sed -i 's/cloud_project:latest/cloud_project:${env.BUILD_ID}/g' hellowhale.yaml"
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "minikubeconfig")
        }
      }
    }

  }

}
