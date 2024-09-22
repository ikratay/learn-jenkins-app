pipeline {
    agent any

    stages {
        stage('Build') {
        // comment
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                    }
                }
        /*
        commentnpm run
        ssssssss
        test
        */
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
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                    }
                }
            steps {
                    echo "Test stage"
                    sh '''
                        test -f build/index.html
                        npm test
                    '''
            }
        }
        stage('Test E2E') {
            agent {
                docker{
                    image 'mcr.microsoft.com/playwright:v1.47.2-noble'
                    reuseNode true
//                     args '-u root:root'
                    }
                }
            steps {
                    echo "Test stage"
                    sh '''
                        npm install -g serve
                        node_modules/.bin/serve -s build &
                        sleep 10
                        npx playwright test --reporter=html
                    '''
            }
        }
    }
    post {
        always {
            junit 'test-results/junit.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'test-results', reportFiles: 'index.html', reportName: 'My result HTML Report', reportTitles: 'junit.xml', useWrapperFileDirectly: true])
        }
    }

}