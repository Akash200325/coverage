pipeline {
    agent any
    
    environment {
        SONAR_SCANNER = tool name: 'SonarQube Scanner', type: 'ToolType'  // Ensure this matches the configured SonarQube Scanner tool
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_PROJECT_KEY = 'coveragepipeline'
        SONAR_TOKEN = 'sqp_93f9cde8ec0e595ce01645c05b71b5d008836293'  // Your SonarQube token
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Your build commands (e.g., compile, test) go here
                    echo 'Build step - Compile or other build tasks'
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
                    -D"sonar.token=${SONAR_TOKEN}"
                    """
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Your testing commands (e.g., unit tests) go here
                    echo 'Test step - Run unit tests'
                }
            }
        }
    }

    post {
        always {
            // Ensure this is inside the node block
            cleanWs()  // Clean workspace after build
        }
    }
}
