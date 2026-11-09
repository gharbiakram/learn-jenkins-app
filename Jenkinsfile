
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
    }
}
