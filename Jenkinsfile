pipeline {
    agent {
        node {
            label 'roboshop'
        }
    }
     environment {
        appVersion = ""
        ACC_ID = "851974187100"
        region = "us-east-1"
    }
    
    options {
        // disableConcurrentBuilds()
        timeout(time: 5, unit: 'MINUTES')
    }

    stages {
        stage('Read version') {
            steps {
                script {
                    // Read package.json into a variable
                    def packageJson = readJSON file: 'package.json'
                    
                    // Set environment variable
                      appVersion = packageJson.version
                    
                    echo "Building version ${env.appVersion}"
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    sh """
                    npm install
                    """               
                }
            }
        }
        
        stage('Build Image') {
            steps {
                script {
                    withAWS(credentials: 'aws-cred', region: "${region}") {
                        // Commands here have AWS authentication
                        sh """
                            aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${ACC_ID}.dkr.ecr.${region}.amazonaws.com/roboshop/catalogue:${appVersion} .
                            docker push ${ACC_ID}.dkr.ecr.${region}.amazonaws.com/roboshop/catalogue:${appVersion}
                        """
                    }
                }
            }
        }
    }
    
    // post build
    post {
        always {
            echo "I will always say hello again"
        }
        success {
            echo "pipeline success"
        }
        failure {
            echo "pipeline failure"
        }
    }
}