pipeline {

  environment {
    dockerimagename = "josefranciscosanchez/frontend"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/JoseFranciscoSanchezGutierrez/devSecOpsFrontend.git'
      }
    }

    stage('Scan') {
        steps {
            snykSecurity organisation: 'josefranciscosanchezgutierrez', projectName: 'devSecOpsFrontend', severity: 'low', snykInstallation: 'Snyk', snykTokenId: 'snyk', targetFile: 'package.json'
        }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend-deployment.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
