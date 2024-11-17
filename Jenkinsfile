pipeline {
    agent any

    stages {
        // This is a comment
        // stage('build') {
        //     agent {
        //         docker {
        //             image 'node:20.18-alpine3.19'
        //             reuseNode true
        //         }
        //     }
        //     steps {
        //         sh '''
        //             ls -la
        //             node --version
        //             npm --version
        //             npm ci 
        //             npm run build
        //             ls -la
        //         '''
        //     }
        // }
        
        stage('test') {
             agent {
                docker {
                    image 'node:20.18-alpine3.19'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    #test -f build/index.html
                    npm test
                '''
            }
        }

        stage('e2e') {
             agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.48.1-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install erve
                    node_modules/.bin/serve -s build
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
