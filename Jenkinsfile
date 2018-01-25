#!groovy

/*
 * Jenkinsfile template for Docker build image to Azure Container Registry.
 *
 */

node {

    /***********************************************************************
     * Constants
     ***********************************************************************/

     def dockerImageName    = "mdf/frontend"
     def dockerRegistry = ""
     def dockerRepository = 'mdf/frontend'
     def dockerCredentialsId = 'docker_credentials'

     //     Change the GitHub repo and git_CredentialsId as per Https or SSH

     def git_repo = "https://github.com/agnnn/dockerized-node.git"
     def git_CredentialsId = "d34217f1-0627-453e-8ba6-6c7b3b5da491"

     /***********************************************************************
      * Builds
      ***********************************************************************/
     stage('Define Environment Variables') {
       echo "  ************************** \
         \n Environment variables used for this Build : \
         \n ***************************" + \
         '\n dockerRegistry: ' + dockerRegistry + \
         '\n dockerRepository:'  + dockerRepository + \
         '\n dockerCredentialsId:'  + dockerCredentialsId + \
         '\n dockerImageName:'  + dockerImageName + \
         '\n dockerImageTag:'  + "${env.BUILD_NUMBER}" + \
         '\n GitHubRepository:' + git_repo + \
         '\n GitHubCredentialsId:' + git_CredentialsId
     }

     stage('Checkout git repo') {
       echo 'Checking out GitHub repository'
       git branch: 'master', url: "${git_repo}", credentialsId: "${git_CredentialsId}"
     }

     stage('Build Docker image') {
       echo 'Build docker image from the GitHub repository '
       built_img = docker.build("${dockerImageName}" + ":${env.BUILD_NUMBER}", '.')
     }

     stage('Push Docker image to Azure Container Registry') {
       echo 'Pushing the built docker image to Azure Container Registry'
       docker.withRegistry("${dockerRegistry}", "docker_credentials") {
         built_img.push("${env.BUILD_NUMBER}");
        }
     }
 }
