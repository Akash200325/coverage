pipeline {
    agent any  // This means the pipeline will use any available agent

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Akash200325/coverage.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'pip install -r requirements.txt'
                }
            }
        }

        stage('Run Unit Tests and Collect Coverage') {
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
            steps {
                script {
                    bat """
                    sonar-scanner -Dsonar.projectKey=coveragepipeline ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=http://localhost:9000 ^
                    -Dsonar.token=sqp_93f9cde8ec0e595ce01645c05b71b5d008836293 ^
                    -Dsonar.python.coverage.reportPaths=coverage_report/coverage.xml
                    """
                }
            }
        }

        stage('Publish Test Results') {
            steps {
                junit '**/test-*.xml'
            }
        }
    }

    post {
        always {
            cleanWs()  // Clean workspace after the build
        }
    }
}
