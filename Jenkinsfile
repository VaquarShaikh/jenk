pipeline {
    agent any
    stages {
        stage('git checkout') {
            steps {
                git credentialsId: 'a462d379-a01f-49b0-a074-9e1a9d579e0a', branch: 'main', url: 'https://github.com/VaquarShaikh/jenk.git'
            }
        }

        stage('install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('code compile') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Run test') {
            steps {
                sh 'npm run e2e:headless'
            }
        }

        stage('sonarqube') {
            steps {
                withSonarQubeEnv('vaqsonar') {
                    sh 'sonar-scanner \
                        -Dsonar.projectKey=jenk \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=sqa_86bdff8efcf3bd343c4f0fb9aca283762c8ab6a7'
                }
            }
        }
    }

// post {
//     always {
//         cleanWs()
//     }
// }
}
