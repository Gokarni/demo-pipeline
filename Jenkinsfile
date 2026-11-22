pipeline{
    agent any
    stages{
        stage("Code Checkout"){
            steps{
                git branch: 'main',
                url: 'https://github.com/Gokarni/demo-pipeline.git'
            }
        }
        stage("Build Image"){
            steps{
                bat 'docker build -t flaskimage .'
            }
        }
        
        stage('AWS Authentication') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'jenkinsecr',
                        usernameVariable: 'AWS_ACCESS_KEY_ID',
                        passwordVariable: 'AWS_SECRET_ACCESS_KEY'
                    )
                ]) {
                    bat '''
                    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 177213405713.dkr.ecr.us-east-1.amazonaws.com/
                    '''
                }
            }
        }
        stage("Push Image"){
            steps{
               bat  '''
                docker tag flaskimage:latest 177213405713.dkr.ecr.us-east-1.amazonaws.com/jenkinsecrdemo:latest
                docker push 177213405713.dkr.ecr.us-east-1.amazonaws.com/jenkinsecrdemo:latest
                '''

            }
        }
    }
}
