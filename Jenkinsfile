pipeline {
      agent {
        node {
            label "primesquare-local"
        }
    }
    
    environment {
        AWS_REGION = 'us-east-2'
        AWS_SECRET_NAME = 'pyama-secret'
        VERSIONSTAGES = 'AWSCURRENT'
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
                   def SECRET_VALUE= sh(script: 'aws secretsmanager get-secret-value --secret-id' + "${AWS_SECRET_NAME} --region ${AWS_REGION}" --query 'SecretString' --output text --version-stage ${VERSIONSTAGES}",
                        returnStdout: true).trim()
                   sh "echo ${SECRET_VALUE}"
                }
            }
        }
     }
 }
