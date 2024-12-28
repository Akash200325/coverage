pipeline {
    agent none  // Define that we do not want to allocate an agent globally for the whole pipeline

    environment {
        SONAR_SCANNER = tool name: 'SonarQube Scanner', type: 'ToolType'
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_PROJECT_KEY = 'coveragepipeline'
        SONAR_TOKEN = 'sqp_93f9cde8ec0e595ce01645c05b71b5d008836293'
        PYTHON_PATH = 'C:/Users/akash/AppData/Local/Programs/Python/Python313/'
    }

    stages {
        stage('Checkout') {
            agent { label 'your-agent-label' }  // Specify the label of the agent to use
            steps {
                git branch: 'main', url: 'https://github.com/Akash200325/coverage.git'
            }
        }

        stage('Install Dependencies') {
            agent { label 'your-agent-label' }  // Specify the label of the agent to use
            steps {
                script {
                    sh 'pip install -r requirements.txt'
                }
            }
        }

        stage('Run Unit Tests and Collect Coverage') {
            agent { label 'your-agent-label' }  // Specify the label of the agent to use
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
            agent { label 'your-agent-label' }  // Specify the label of the agent to use
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
            agent { label 'your-agent-label' }  // Specify the label of the agent to use
            steps {
                junit '**/test-*.xml'
            }
        }
    }

    post {
        always {
            node {
                cleanWs()
            }
        }
    }
}
