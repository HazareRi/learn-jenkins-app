pipeline {
    agent any

    environment {
        // Add your Netlify Auth Token securely in Jenkins credentials with the ID 'netlify-auth-token'
        NETLIFY_AUTH_TOKEN = credentials('nfp_gbLqF2Rq3MofAYEV2qKCbLSyZXMQjx6o4294') // Ensure this is correct and corresponds to your stored token
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.image('node:18-alpine').inside {
                        sh 'node --version'
                        sh 'npm --version'
                        sh 'npm ci'
                        sh 'npm run build'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image('node:18-alpine').inside {
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Deploy to Netlify') {
            steps {
                script {
                    docker.image('node:18-alpine').inside {
                        // Netlify deployment using the token from the environment
                        sh '''
                        netlify deploy --dir=build --prod --auth=$NETLIFY_AUTH_TOKEN
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            node('master') { // Add label to node block, for example, 'master' or use your own agent label
                junit 'test-results/*.xml'
            }
        }
    }
}
