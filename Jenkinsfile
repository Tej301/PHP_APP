pipeline{
    agent any
    environment{
        IMAGE_NAME ='devopstrainer/java-mvn-privaterepos:php$BUILD_NUMBER'
        SERVER_IP ='ec2-user@13.126.50.89'
        TEST_SERVER_IP ='ec2-user@13.126.50.89'
    }
    stages{
        stage('Build docker image and run docker-compose'){
            agent any
             steps{
                script{
        sshagent(['DEPLOY_SERVER']) { 
        withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
    echo "Building the docker image"
    sh "scp -o StrictHostKeyChecking=no -r docker-files ${SERVER_IP}:/home/ec2-user"
    sh "ssh -o StrictHostKeyChecking=no ${SERVER_IP} 'bash ~/docker-files/docker-script.sh'"
    sh "ssh ${SERVER_IP} sudo docker build -t ${IMAGE_NAME} /home/ec2-user/docker-files/"
    sh "ssh ${SERVER_IP} sudo docker login -u $USERNAME -p $PASSWORD"
    sh "ssh ${SERVER_IP} sudo docker push ${IMAGE_NAME}"
    sh "ssh ${SERVER_IP} bash /home/ec2-user/docker-files/docker-compose-script.sh ${IMAGE_NAME}"
      
                 }
            }
        }
         }
}
    }
}