pipeline {
    agent any
    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/Ashadow9/php-project', branch: 'master'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t skywalkerdarth/27sep:v1 .'
                    sh 'docker images'
                }
            }
        }
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push skywalkerdarth/27sep:v1'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def dockerrm = 'sudo docker rm -f My-first-container2211 || true'
                    def dockerCmd = 'sudo docker run -itd --name My-first-container2211 -p 8083:80 skywalkerdarth/27sep:v1'
                    sshagent(['sshkeypair']) {
                        // Update the private IP in the below command
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.47.97 ${dockerrm}"
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.47.97 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
