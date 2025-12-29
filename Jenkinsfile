pipeline {
    agent any
    stages {
        stage('Install Tools') {
            steps {
                sh '''
                python3 --version
                pip3 install --user pytest pytest-cov
                '''
            }
        }
        stage('Run Tests with Coverage') {
            steps {
                sh '~/.local/bin/pytest --cov=. --cov-report=term > coverage.txt'
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
