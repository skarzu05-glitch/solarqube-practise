pipeline {
    agent { label 'ubuntu-agent' }

    stages {
        stage('Tests and coverage') {
            steps {
                sh '''
                    python3 -m venv .venv
                    .venv/bin/python -m pip install coverage
                    .venv/bin/python -m coverage run --branch --source=calculator -m unittest discover -v
                    .venv/bin/python -m coverage xml -o coverage.xml
                    .venv/bin/python -m coverage report
                '''
            }
        }

        stage('SonarQube analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'

                    withSonarQubeEnv('sonarqube') {
                        withEnv(["SCANNER_HOME=${scannerHome}"]) {
                            sh '"$SCANNER_HOME/bin/sonar-scanner"'
                        }
                    }
                }
            }
        }
    }
}
