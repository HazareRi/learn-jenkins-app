pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                   ls -la
                   node --version
                   npm --version
                   npm ci
                   npm run build
                   ls -la
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                test -f build/index.html  # Check if the build was successful
                npm test  # Run your test suite
                '''
            }
        }

        stage('Deploy to Netlify') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            environment {
                NETLIFY_AUTH_TOKEN = credentials('netlify-auth-token')  // Fetching token securely
            }
            steps {
                sh '''
                 npm install netlify-cli
                 npx netlify --version  # Verify the Netlify CLI installation
                 npx netlify deploy --prod --dir=build  # Specify the build directory
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'  // Collect test results if available
        }
    }
}
