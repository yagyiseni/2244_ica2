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
                sh 'sudo docker run -d -p 8081:80 rohan2053/devopssecond:latest'
            } 
        }


        

        stage('testing') {
            steps {
                sh 'curl -I 3.86.83.163:8081'
            }
       }

       stage('Build and Push') {
            steps {
                echo 'Building..'
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