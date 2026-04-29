pipeline {
    agent {
        node {
            label 'roboshop'
        }
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
                    env.appVersion = packageJson.version
                    
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
                    sh """
                    docker build -t catalogue:${env.appVersion} .
                    """
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