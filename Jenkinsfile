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
                test -f build/index.html   # Check if index.html is present after build
                npm test
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
            steps {
                sh '''
                 npm install netlify-cli -g
                 netlify --version
                 netlify deploy --prod   # Deploying to Netlify
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'  # Always publish test results
        }
    }
}
