/*
 * Template Jenkinsfile for Docker build image to Azure Container Registry.
 * For this template to work you need some custom
 */

 Jenkinsfile (Declarative Pipeline)
 pipeline {
    node {
    def built_img = ''
    stage('Checkout git repo') {
      git branch: 'develop', url: params.git_repo, credentialsId: params.git_credentials}
    stage('Build Docker image') {
      built_img = docker.build(params.docker_repository + ":${env.BUILD_NUMBER}", '.')
    }
    stage('Push Docker image to Azure Container Registry') {
      docker.withRegistry(params.registry_url, params.registry_credentials_id ) {
        built_img.push("${env.BUILD_NUMBER}");
      }
    }
}
}
