pipeline {
    agent any
    stages {
        stage('Create Venv') {
            steps {
                sh '''
                python3 -m venv venv
                ./venv/bin/pip install --upgrade pip
                ./venv/bin/pip install pytest pytest-cov
                '''
            }
        }
        stage('Run Tests with Coverage') {
            steps {
                sh './venv/bin/pytest --cov=. --cov-report=term > coverage.txt'
            }
        }
        stage('Fail if Coverage < 80%') {
            steps {
                script {
                    def coverage = sh(
                        script: "grep TOTAL coverage.txt | awk '{print \$4}' | tr -d '%'",
                        returnStdout: true
                    ).trim()
                    echo "Coverage = ${coverage}%"
                    if (coverage.toInteger() < 80) {
                        error "Coverage ${coverage}% is below 80%"
                    }
                }
            }
        }
    }
}
