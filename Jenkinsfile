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
                SONAR_SCANNER_FOLDER = 'sonar-scanner'
                JAVA_17_PATH = '"C:\\Program Files\\Java\\jdk-17\\bin\\java.exe"'
            }
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    bat """
                        curl -o sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.8.0.2856-windows.zip
                        powershell -Command "Expand-Archive -Force sonar-scanner.zip %SONAR_SCANNER_FOLDER%"
                        %JAVA_17_PATH% -jar %SONAR_SCANNER_FOLDER%\\sonar-scanner-4.8.0.2856-windows\\lib\\sonar-scanner-cli-4.8.0.2856.jar -D"sonar.login=%SONAR_TOKEN%"
                    """
                }
            }
        }
    }
}






