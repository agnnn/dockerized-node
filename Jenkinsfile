/*
 * Template Jenkinsfile for Docker build image to Azure Container Registry.
 * For this template to work you need some custom
 */

 pipeline {
     agent none

     options {
       disableConcurrentBuilds()
     }

     parameters {
           string(name: 'git_repo', defaultValue:"git@github.com:agnnn/dockerized-node.git", description: "GitHub repository")
           string(name: 'docker_repository', defaultValue:"mdf/frontend", description: "Docker repository")
           string(name: 'registry_url', defaultValue:"https://ofoovmmfpfoss.azurecr.io", description: "Azure Docker repository")
       }

    stages {
      def built_img = ''

      stage('Checkout git repo') {
        git branch: 'develop', url: params.git_repo, credentialsId: d34217f1-0627-453e-8ba6-6c7b3b5da491}


      stage('Build Docker image') {
        built_img = docker.build(params.docker_repository + ":${env.BUILD_NUMBER}", '.')
      }


      stage('Push Docker image to Azure Container Registry') {
        docker.withRegistry(params.registry_url, credentialsId: docker_credentials ) {
          built_img.push("${env.BUILD_NUMBER}");
        }
      }

      post {
        failure {
          // notify users when the Pipeline fails
          mail to: 'ravindran@pythian.com',
              subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
              body: "Something is wrong with ${env.BUILD_URL}"
        }

     }
   }
  }
