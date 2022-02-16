pipeline{
    agent any
    environment{
        IMAGE_NAME ='devopstrainer/java-mvn-privaterepos:php$BUILD_NUMBER'
    }
    stages{
        stage('Build docker image'){
            agent any
             steps{
                script{
        sshagent(['Test_server-Key']) {
        withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
    echo "Building the docker image"
    sh "scp -o StrictHostKeyChecking=no docker-script.sh ec2-user@172.31.33.78:/home/ec2-user"
    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.33.78 'bash ~/docker-script.sh'"
    sh "ssh ec2-user@172.31.33.78 sudo docker build -t ${IMAGE_NAME} /home/ec2-user/php-1"
    sh "ssh ec2-user@172.31.33.78 sudo docker login -u $USERNAME -p $PASSWORD"
    sh "ssh ec2-user@172.31.33.78 sudo docker push ${IMAGE_NAME}"
                 }
            }
        }
         }
}
    }
}