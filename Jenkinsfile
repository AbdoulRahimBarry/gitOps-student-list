pipeline {
    agent any
    environment{
        /* Variable */
        PROJECT_ID = 'infrastructure-176114'   /* Identifiand Project Google Cloud Platform*/
        IMAGE_NAME = 'voyancegratuite'    /*Image Name*/
        HOSTNAME = 'eu.gcr.io'          /*Host Name*/

    }
    
    stages{
        stage('Build and Push Docker Image'){
             steps {
                 script {
                    /* Build the image to the google container Registry form */
                    /*Login to the Google Container resistry, [service_account_json_key: key creact to the Jenkins credential]*/
                    
                    docker.withRegistry('https://eu.gcr.io','gcr:service_account_json_key') {
                        def containerResistry = docker.build("${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.GIT_COMMIT}"," -f Dockerfile .")
                        
                        sh "docker push ${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.GIT_COMMIT}"

                     }
                 }
             }
        }

        stage('Deploy in Staging E2E') {
             steps {
                 /*pull the repository in Github using SSH, [github-key: Private key creact to Jenkins credential]*/
                 git credentialsId: 'github-key', url: 'git@github.com:AbdoulRahimBarry/kustomaze-demo.git'


                 dir("e2e"){
                     /*Use the kustomaze to change image tag*/
                     sh "kustomize edit set image ${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.GIT_COMMIT}"
                     
                     /*Use the sshagent to push the manifest in Github*/
                     sshagent(['github-key']){
                       sh "pwd"
                       sh "git remote -v && git status"
                       sh "git add kustomization.yaml"
                       sh "git commit -m 'Publish new version'"
                       sh 'git push -u origin master'
                     }
                 }
             }
        }
        

    }
}
