pipline{
   agent any
   environment{
       DOCKERHUB_USERNAME = 'sahanata91'
       APP_NAME = 'gitops-demo-app'
       IMAGE_TAG ='$(BUILD_NUMBER)'
       IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
       REGISTRY_CREDS = 'dockerhub' 
   }

   stages{
       stage('cleanup workspace'){
         steps{
             script{
                 cleanWs()
                }
             }
       }    
    stage('Checkout SCM'){
            steps {
                git credentialsId: 'github',
                url: 'https://github.com/sahana-m-a/gitops-demo.git'
                branch: 'master'
                 }
        }
    stage('Build Docker Image'){
            steps {
                script{
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Push Docker Image'){
            steps {
                script{
                    docker.withRegistry('', REGISTRY_CREDS ){
                        docker_image.push("${BUILD_NUMBER}")
                        docker_image.push('latest')
                    }
                }
            }
        } 
         stage('Delete Docker Images'){ 
            steps {
                sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG} ."
                sh "docker rmi ${IMAGE_NAME}:latest ."
                }
        }
         
   }
}


// stage('Build Docker Image'){
//             steps {
//                 sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
//                 sh "docker build -t ${IMAGE_NAME}:latest ."
//             }
//         }
//         stage('Push Docker Image'){
//             steps {
//                 withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: 'user')]) {
//                     sh "docker login -u $user --password $pass"
//                     sh "docker push ${IMAGE_NAME}:${IMAGE_TAG} ."
//                     sh "docker push ${IMAGE_NAME}:latest ."
//                 }
//             }
//         }
