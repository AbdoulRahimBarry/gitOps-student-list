pipeline {
    agent any
    environment{
        /* Variable */
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
                        def containerResistry = docker.build("${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.GIT_COMMIT}"," -f simple_api/Dockerfile .")
                        
                        //sh "docker tag ${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.GIT_COMMIT} ${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.GIT_COMMIT}"
                        sh "docker push ${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.GIT_COMMIT}"

                     }
                 }
             }
        }


        stage('Deploy E2E') {
             steps {
                 //git credentialsId: 'git_credential', url: 'https://github.com/AbdoulRahimBarry/kustomaze-demo.git'
                 git credentialsId: 'git', url: 'https://github.com/AbdoulRahimBarry/kustomaze-demo'
                 sh "pwd"
                 
                 /* Cloning the project */
                 //sh "git clone https://github.com/AbdoulRahimBarry/kustomaze-demo.git "
                 
                 //sh "cd ./e2e && kustomize edit set image ${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.GIT_COMMIT}"
                 //sh "git commit -am 'Publish new version' && git push --set-upstream origin master || echo 'no changes'"
                 dir('e2e'){
                     sh "kustomize edit set image ${env.HOSTNAME}/${env.PROJECT_ID}/${env.IMAGE_NAME}:${env.GIT_COMMIT}"
                     sh "pwd"
                     //sh "git status && git add kustomization.yaml && git commit -m 'Publish new version' && git status && git remote -v && git push"
                     sshagent (credentials: ['github-key']) {
                       sh "git status"
                     }
                 }

             }
        }
        

    }
}
