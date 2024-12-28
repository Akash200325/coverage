pipeline {
    agent none  // This allows you to specify the node in individual stages

    environment {
        SONAR_SCANNER = tool name: 'sonarqube', type: 'ToolType'  // Ensure this matches the configured SonarQube Scanner in Jenkins
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_PROJECT_KEY = 'coveragepipeline'
        SONAR_TOKEN = 'sqp_93f9cde8ec0e595ce01645c05b71b5d008836293'  // Your SonarQube token
        PYTHON_PATH = 'C:/Users/akash/AppData/Local/Programs/Python/Python313/'  // Use forward slashes or double backslashes
    }

    stages {
        stage('Checkout') {
            agent { label 'your-agent-label' }  // Specify the label of the agent to use
            steps {
                // Explicitly specifying the GitHub repository URL
                git branch: 'main', url: 'https://github.com/Akash200325/coverage.git'
            }
        }

        stage('Install Dependencies') {
            agent { label 'your-agent-label' }  // Specify the label of the agent to use
            steps {
                script {
                    // Install required dependencies (assuming requirements.txt exists for Python)
                    sh 'pip install -r requirements.txt'
                }
            }
        }

        stage('Run Unit Tests and Collect Coverage') {
            agent { label 'your-agent-label' }  // Specify the label of the agent to use
            steps {
                script {
                    // Run unit tests and collect coverage
                    sh '''
                    coverage run -m unittest discover -s tests -p "test_*.py"  # Adjust if your test directory or pattern differs
                    coverage report  # Display coverage summary in console
                    coverage html -d coverage_report  # Generate HTML coverage report
                    '''
                }
            }
        }

        stage('SonarQube Analysis') {
            agent { label 'your-agent-label' }  // Specify the label of the agent to use
            steps {
                script {
                    // Run SonarQube analysis using the scanner
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
                junit '**/test-*.xml'  // Adjust based on your test result XML format
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
