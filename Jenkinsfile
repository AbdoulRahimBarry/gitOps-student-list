pipeline {
    agent any
    environment{
        /* Variable */
        PROJECT_ID = 'traefikproject'   /* Identifiand Project Google Cloud Platform*/
        IMAGE_NAME = 'pozos-website'    /*Image Name*/
        HOSTNAME = 'eu.gcr.io'          /*Host Name*/

    }
    
    stages{
        stage('Build and Push Docker Image'){
             steps {
                 script {
                    /* Build the image to the google container Registry form */
                    /*Login to the Google Container resistry, [service_account_json_key: key creact to the Jenkins credential]*/
                    docker.withRegistry('https://eu.gcr.io','gcr:service_account_json_key') {
                        def containerResistry = docker.build("${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.GIT_COMMIT}"," -f simple_api/Dockerfile .")
                        
                        sh "docker push ${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.GIT_COMMIT}"

                     }
                 }
             }
        }
        

    }
}
