pipeline {
    agent any
    environment{
        /* Variable */
        DOCKER_TAG = getDockerTag()
        PROJECT_ID = 'traefikproject'
        IMAGE_NAME = 'pozos-website' //CONTAINER
        HOSTNAME = 'eu.gcr.io'

        ARGOCD_SERVER='35.188.203.1'

    }
    
    stages{
        stage('Build Docker Image'){
             steps {
                 script {
                    /* Build the image to the google container Registry form */
                    docker.withRegistry('https://eu.gcr.io','gcr:service_account_json_key') {
                        def containerResistry = docker.build("${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.BUILD_ID}"," -f simple_api/Dockerfile .")
                        
                        sh "docker tag ${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME} ${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.DOCKER_TAG}"
                        sh "docker push ${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.DOCKER_TAG}"
                         
                        /* Push the image to the google container Registry */
                        //stage 'push image'
                        //containerResistry.push('latest')

                     }
                 }
             }
        }
        stage('Deploy Docker Image with Argocd'){
             steps {
               withCredentials([string(credentialsId: 'argocd', variable: 'ARGOCD_AUTH_TOKEN')]) {
                 sh """
                 docker image inspect ${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.DOCKER_TAG} -f '{{join .RepoDigests ","}}')
                 """
               }
             }
        }
    }
}

/* Permet to tag image with the id commit, but don't user this code  */
def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
