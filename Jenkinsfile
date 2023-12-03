pipeline{
    agent any
    
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'javahome2', url: 'https://github.com/amrithaws/onlinebookstore.git'
            }
        }
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
                sh "mv target/*.war target/Onlinebookstore.war"
            }
        }
        stage("deploy-dev"){
            steps{
                sshagent(['tomcat']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/Onlinebookstore.war  ec2-user@172.31.34.147:/home/ec2-user/apache-tomcat-9.0.83/webapps/
                    
                    ssh ec2-user@172.31.34.147 /home/ec2-user/apache-tomcat-9.0.83/bin/shutdown.sh
                    
                    ssh ec2-user@172.31.34.147 /home/ec2-user/apache-tomcat-9.0.83/bin/startup.sh
                
                """
            }
            
            }
        }
    }
}
