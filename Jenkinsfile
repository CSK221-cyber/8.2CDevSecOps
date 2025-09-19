pipeline {
  agent any

  environment {
    SONAR_TOKEN = credentials('SONAR_TOKEN')
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/CSK221-cyber/8.2CDevSecOps.git'
      }
    }
    
    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }
    
    stage('Run Tests') {
      steps {
        sh 'npm test || true'
      }
    }
    
    stage('Generate Coverage Report') {
      steps {
        sh 'npm run coverage || true'
      }
    }
    
    stage('NPM Audit (Security Scan)') {
      steps {
        sh 'npm audit || true'
      }
    }

    stage('SonarCloud Analysis') {
      steps {
        // Download SonarScanner CLI
        sh '''
          curl -sSLo sonar-scanner-cli.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.8.0.2856-linux.zip
          unzip sonar-scanner-cli.zip
          ./sonar-scanner-4.8.0.2856-linux/bin/sonar-scanner
        '''
      }
    }
  }
}
