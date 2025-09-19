pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'https://sonarcloud.io'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/CSK221-cyber/8.2CDevSecOps.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
        }
        
        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }
        
        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }

        stage('SonarCloud Analysis') {
            environment {
                SONAR_TOKEN = credentials('SONAR_TOKEN') // Securely load your token from Jenkins credentials
            }
            steps {
                script {
                    bat '''
                    powershell -Command "Invoke-WebRequest -Uri https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.8.0.2856-windows.zip -OutFile sonar-scanner.zip"
                    powershell -Command "Expand-Archive -Path sonar-scanner.zip -DestinationPath . -Force"
                    sonar-scanner-4.8.0.2856-windows\\bin\\sonar-scanner.bat ^
                        -Dsonar.projectKey=CSK221-cyber_8.2CDevSecOps ^
                        -Dsonar.organization=CSK221-cyber ^
                        -Dsonar.sources=. ^
                        -Dsonar.exclusions=node_modules/**,test/** ^
                        -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info ^
                        -Dsonar.projectName="8.2CDevSecOps" ^
                        -Dsonar.sourceEncoding=UTF-8 ^
                        -Dsonar.host.url=%SONAR_HOST_URL% ^
                        -Dsonar.login=%SONAR_TOKEN%"
                    '''
                }
            }
        }
    }
}
