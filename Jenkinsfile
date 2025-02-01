pipeline {
    agent any

    tools {
        nodejs 'NodeJS-22-6-0'
    }

    environment {
        MONGO_URI = "mongodb+srv://supercluster.d83jj.mongodb.net/superData"
        }

    stages {
        stage('Installing Dependencies') {
            steps {
                sh 'npm install --no-audit'
            }
        }

        stage('NPM Dependency Audit') {
            steps {
                sh '''
                    npm audit --audit-level=critical
                    echo $?
                '''
            }
        }

        stage('Unit Testing') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'MONG0_DB', passwordVariable: 'MONGO_PASSWORD', 
                usernameVariable: 'MONGO_USERNAME')]) {
                    sh 'npm test'
                }

                junit allowEmptyResults: true, stdioRetention: '', testResults: 'test-results.xml'
            }
        }
    }
}


