pipeline {
    agent none  // This means the pipeline itself doesn't allocate a global agent

    stages {
        stage('Checkout') {
            agent { label 'master' }  // Specify which node/agent to use here
            steps {
                git branch: 'main', url: 'https://github.com/Akash200325/coverage.git'
            }
        }

        stage('Install Dependencies') {
            agent { label 'master' }  // Specify which node/agent to use here
            steps {
                script {
                    sh 'pip install -r requirements.txt'
                }
            }
        }

        stage('Run Unit Tests and Collect Coverage') {
            agent { label 'master' }  // Specify which node/agent to use here
            steps {
                script {
                    sh '''
                    coverage run -m unittest discover -s tests -p "test_*.py"
                    coverage report
                    coverage html -d coverage_report
                    '''
                }
            }
        }

        stage('SonarQube Analysis') {
            agent { label 'master' }  // Specify which node/agent to use here
            steps {
                script {
                    bat """
                    ${SONAR_SCANNER} -D"sonar.projectKey=${SONAR_PROJECT_KEY}" \
                    -D"sonar.sources=." \
                    -D"sonar.host.url=${SONAR_HOST_URL}" \
                    -D"sonar.token=${SONAR_TOKEN}" \
                    -D"sonar.python.coverage.reportPaths=coverage_report/coverage.xml"
                    """
                }
            }
        }

        stage('Publish Test Results') {
            agent { label 'master' }  // Specify which node/agent to use here
            steps {
                junit '**/test-*.xml'
            }
        }
    }

    post {
        always {
            node('master') {  // Use the same node label here
                cleanWs()
            }
        }
    }
}
