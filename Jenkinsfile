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
                sh '/home/vaq/.nvm/versions/node/v22.11.0/bin/npm install'
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

        stage('SonarQube Analysis') {
            steps{
                sh '''
                sonar-scanner \
                -Dsonar.projectKey=jenkk \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://localhost:9000 \
                -Dsonar.token=sqp_3bfe1c645b5db2ee90bb7b325f630a03de696b0a
                '''
            }
        }
    }
    // post {
    //     always {
    //         cleanWs()
    //     }
    // }
}
