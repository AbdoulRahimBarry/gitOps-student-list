pipeline {
    agent any
    environment{
        /* Variable */
        DOCKER_TAG = getDockerTag()
        PROJECT_ID = 'traefikproject'
        IMAGE_NAME = 'pozos-website' //CONTAINER
        HOSTNAME = 'eu.gcr.io'

        ARGOCD_SERVER='35.188.203.1'
        APP_NAME="debian-test-k8s"

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

                     }
                 }
             }
        }


        stage('Deploy E2E') {
             steps {
                 git credentialsId: 'git_credential', url: 'https://github.com/AbdoulRahimBarry/kustomaze-demo.git'
                 
                 /* Cloning the project */
                 sh "git clone https://github.com/AbdoulRahimBarry/kustomaze-demo.git "
                 
                 //sh "cd ./e2e && kustomize edit set image ${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.DOCKER_TAG}"
                 //sh "git commit -am 'Publish new version' && git push || echo 'no changes'"
             }
        }
        

    }
}

/* Permet to tag image with the id commit, but don't user this code  */
def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
