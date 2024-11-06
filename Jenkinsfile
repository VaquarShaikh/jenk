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
                /opt/sonar-scanner/bin/sonar-scanner \
                -Dsonar.projectKey=jenkk \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://localhost:9000 \
                -Dsonar.token=sqp_3bfe1c645b5db2ee90bb7b325f630a03de696b0a
                '''
            }
        }

        // stage('OWASP ZAP API Key Generation') {
        //     steps {
        //         sh 'openssl rand -hex 16 > zap_api_key.txt'
        //     }
        // }

        stage('Owasp'){
            steps{
                sh 'zap-cli --api-key $(cat zap_api_key.txt) ajax-spider http://localhost:3000'
            }
        }

        stage('Owasp report'){
            steps{
                sh 'zap-cli --api-key $(cat zap_api_key.txt) report -o zap_report.html -f html'
            }
        }

        stage('push to nexus'){
            steps{
                // Define the path to the file and Nexus credentials
                sh '''
                curl -u admin:vaq -X POST "http://localhost:8081/service/rest/v1/components?repository=vaq-test" \
                -F "raw.directory=/" \
                -F "raw.asset1=@jenk-1.0.0.tgz" \
                -F "raw.asset1.filename=jenk-1.0.0.tgz"
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
