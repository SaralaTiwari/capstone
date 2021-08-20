pipeline {
    agent any
    
    stages {
//        stage('Checkout') {
//            steps {
//                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nickb210/capstone']]])
//            }
//        }
        stage('Test') {
            steps {
                echo "**********************************************"
                echo "              TEST"
                echo "**********************************************"
                sh "pwd"
                sh "ls -al"

                dir('deploy') {
                    sh "pwd"
                    sh "ls -al"
                }
                sh "pwd"
                sh "ls -al"
                sh "ls -alh ${env.PRIVATE_KEY}"

            }
        }

        stage('Pull new image') {
            steps {
                echo "**********************************************"
                echo "              TEST"
                echo "**********************************************"

                sh "chmod 400 ${env.PRIVATE_KEY}"

                dir ('/Users/nicholausbrell/Desktop/capstone/deploy/') {
                    sh "${env.TERRAFORM_HOME}/terraform output instance_aws_eip > ec2_ip"
                    sh "cat ec2_ip"

                    sh ("""sed -i -e "s/\\./-/g" ec2_ip""")
                    sh """sed -i -e "s/\\"//g" ec2_ip"""
                    sh '''export EC2_IP=$(cat ec2_ip)'''

                    sh """ssh -i ${env.PRIVATE_KEY} ec2-user@ec2-52-202-178-179.compute-1.amazonaws.com \"sudo docker pull nickb09/capstone:latest\" """
                    sh """ssh -i ${env.PRIVATE_KEY} ec2-user@ec2-52-202-178-179.compute-1.amazonaws.com \"sudo docker run -p 80:80 nickb09/capstone:latest\" """
                    
                }
            }
        }

    }
}