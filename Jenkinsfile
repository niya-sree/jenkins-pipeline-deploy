pipeline {
    agent any

/*    
    triggers {
        pollSCM('* * * * *')
    }
*/

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image...'
            
                sh 'docker build -t nginx-app:latest .'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Nginx application...'
       
                sh '''

                    docker stop web-app || true
                    docker rm web-app || true
                    docker run -d --name web-app -p 8081:80 nginx-app:latest

                '''
            }
        }

        stage('Verify') {
            steps {
                echo 'Verifying deployment...'
   
                sh '''
                    sleep 5
                    docker ps | grep web-app
                '''
            }
        }
    }
     
    post {
        success {
            echo 'Nginx application deployed successfully!'
        }
        failure {
            echo 'deployment failed'
        }
    }
}
     

          

