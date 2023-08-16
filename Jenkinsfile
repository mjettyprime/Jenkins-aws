pipeline {
      agent {
        node {
            label "primesquare-local"
        }
    }
    
    environment {
        AWS_REGION = 'us-east-2'
        AWS_SECRET_NAME = 'pyama-secret'
    }
    
    stages {
        stage('Retrieve AWS Credentials') {
            steps {
                script {
                   withCredentials([[
            $class: 'AmazonWebServicesCredentialsBinding',
            credentialsId: 'AWS',
            accessKeyVariable: 'AWS_ACCESS_KEY_ID',
            secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
        ]]) {
                        sh '''
                            export AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
                            export AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
                        '''
                    }
                }
            }
        }
        
        stage('Retrieve AWS Secret') {
            steps {
                script {
                    def awsSecret = sh(
                        script: "aws secretsmanager get-secret-value --secret-id ${AWS_SECRET_NAME} --region ${AWS_REGION}",
                        returnStdout: true
                    ).trim()
                    
                    def secretValue = secretJson.SecretString
                    sh 'echo ${secretvalue}'
                }
            }
        }
     }
 }
