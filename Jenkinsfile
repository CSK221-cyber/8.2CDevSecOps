pipeline {
  agent any

  environment {
    SONAR_TOKEN = credentials('SONAR_TOKEN')
  }

  stages {
    stage('Checkout') {
      steps {
        bat 'git clone -b main https://github.com/your_github_username/8.2CDevSecOps.git .'
      }
    }
    
    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }
    
    stage('Run Tests') {
      steps {
        // continue even if tests fail
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
      steps {
        bat '''
          powershell -Command "Invoke-WebRequest -Uri https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.8.0.2856-windows.zip -OutFile sonar-scanner-cli.zip"
          powershell -Command "Expand-Archive -Path sonar-scanner-cli.zip -DestinationPath ."
          sonar-scanner-4.8.0.2856-windows\\bin\\sonar-scanner.bat
        '''
      }
    }
  }
}
