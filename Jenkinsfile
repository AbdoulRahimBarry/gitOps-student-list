pipeline {
    agent any
    environment{
        /* Variable */
        DOCKER_TAG = getDockerTag()
        PROJECT_ID = 'traefikproject'
        IMAGE_NAME = 'pozos-website'
    }
    
    stages{
        stage('Build Docker Image'){
             steps {
                 script {
                    /* Build the image to the google container Registry form */
                    docker.withRegistry('https://eu.gcr.io','gcr:service_account_json_key') {
                        def containerResistry = docker.build("${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.BUILD_ID}"," -f simple_api/Dockerfile .")
                         
                        /* Push the image to the google container Registry */
                        stage 'push image'
                        containerResistry.push('latest')

                     }
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
