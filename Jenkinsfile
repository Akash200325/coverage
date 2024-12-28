pipeline {
    agent any

    environment {
        // Define SonarQube parameters
        SONARQUBE_SCANNER = tool name: 'SonarQube Scanner', type: 'ToolType'
        SONAR_TOKEN = 'sqp_93f9cde8ec0e595ce01645c05b71b5d008836293' // Replace with your SonarQube token
        SONAR_URL = 'http://localhost:9000'
        SONAR_PROJECT_KEY = 'coveragepipeline'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                git branch: 'main', url: 'https://github.com/Akash200325/coverage.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                script {
                    // Create virtual environment and install dependencies
                    sh 'python -m venv venv'
                    sh './venv/Scripts/activate' // For Windows
                    // For Linux/Mac: sh 'source venv/bin/activate'
                    sh 'pip install -r requirements.txt' // Install dependencies from requirements.txt
                }
            }
        }

        stage('Run Tests and Collect Coverage') {
            steps {
                script {
                    // Run unit tests and collect coverage
                    sh 'coverage run -m unittest discover -s . -p "test_*.py"'
                    sh 'coverage report'  // Display the coverage report in the console
                    sh 'coverage html'    // Generate the HTML report
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis using sonar-scanner
                    withSonarQubeEnv('SonarQube') {
                        sh """ 
                        sonar-scanner \
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=${SONAR_URL} \
                        -Dsonar.token=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }

        stage('Publish Coverage Report') {
            steps {
                script {
                    // Publish the HTML coverage report as a Jenkins artifact
                    archiveArtifacts artifacts: 'htmlcov/**', allowEmptyArchive: true
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Clean up virtual environment
                    sh 'rm -rf venv'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
        }
        success {
            echo 'Tests passed and coverage collected successfully!'
        }
        failure {
            echo 'Tests failed or coverage collection failed!'
        }
    }
}
