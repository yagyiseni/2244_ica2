pipeline {
    agent any
    stages {
        stage('Cleanup') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Git Repo') {
            steps {
                checkout scm
            }
        }
        stage('Clone from repository') {
            steps {
                git url: 'https://github.com/yagyiseni/2244_ica2.git', branch: 'develop'
            }
        }

        stage('Build and run docker image') {
            steps {
                sh 'sudo docker build -t rohan2053/devopssecond:latest .'
                sh "sudo docker tag rohan2053/devopssecond:latest rohan2053/devopssecond:develop-${env.BUILD_ID}"
                sh 'sudo docker stop rohan-app-one || true && sudo docker rm rohan-app-one || true' 
                sh 'sudo docker run --name rohan-app-one -d -p 9001:80 rohan2053/devopssecond:latest'
            } 
        }


        

        stage('testing') {
            steps {
                sh 'curl -I 3.95.132.227:9001'
            }
       }

       stage('Build and Push') {
            steps {
                echo 'Building..(7th dec)'
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-auth', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            sudo docker login -u ${USERNAME} -p ${PASSWORD}
                            sudo docker push rohan2053/devopssecond:latest
                        '''
                        sh "sudo docker push rohan2053/devopssecond:develop-${env.BUILD_ID}"
                    }
            }
        }


}
}