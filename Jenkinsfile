
pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'df5641f0-7aa5-444d-be33-28dfabfabe70'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }
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
                    echo "=== Project files ==="
                    ls -la

                    echo "=== Environment ==="
                    node --version
                    npm --version

                    echo "=== Installing dependencies ==="
                    npm ci

                    echo "=== Building project ==="
                    npm run build

                    echo "=== Build output ==="
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
                    echo "=== Running tests ==="
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    echo "=== Deploying project ==="
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying to Netlify site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                '''
            }
        }
    }
}
