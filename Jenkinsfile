pipeline {
    agent any

    environment {
        SONAR_SCANNER = tool name: 'Sonarqube', type: 'ToolType'  // Ensure this matches the configured SonarQube Scanner in Jenkins
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_PROJECT_KEY = 'coveragepipeline'
        SONAR_TOKEN = 'sqp_93f9cde8ec0e595ce01645c05b71b5d008836293'  // Your SonarQube token
        PYTHON_PATH = '/usr/bin/python3'  // Ensure this points to your Python installation (if using Python)
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Checkout the code from GitHub
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install required dependencies (assuming requirements.txt exists for Python)
                    sh 'pip install -r requirements.txt'
                }
            }
        }

        stage('Run Unit Tests and Collect Coverage') {
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
            steps {
                junit '**/test-*.xml'  // Adjust based on your test result XML format
            }
        }
    }

    post {
        always {
            // Clean workspace after build
            cleanWs()
        }
    }
}
