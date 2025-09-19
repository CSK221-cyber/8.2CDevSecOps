pipeline {
 agent any
 stages {
 stage('Checkout') {
 steps {
 git branch: 'main', url: ' https://github.com/CSK221-cyber/8.2CDevSecOps.git'
 }
 }
 stage('Install Dependencies') {
 steps {
 sh 'npm install'
 }
 }
 stage('Run Tests') {
 steps {
 sh 'npm test || true' // Allows pipeline to continue despite test failures
 }
 }
 stage('Generate Coverage Report') {
 steps {
 // Ensure coverage report exists
 sh 'npm run coverage || true'
 }
 }
 stage('NPM Audit (Security Scan)') {
 steps {
 sh 'npm audit || true' // This will show known CVEs in the output
 }
 }
 stage('SonarCloud Analysis') {
  environment {
    SONAR_TOKEN = credentials('SONAR_TOKEN')
  }
  steps {
    sh '''
      wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.8.0.2856-linux.zip
      unzip -o sonar-scanner-cli-4.8.0.2856-linux.zip
      ./sonar-scanner-4.8.0.2856-linux/bin/sonar-scanner -Dsonar.login=$SONAR_TOKEN
    '''
  }
 }
 }
}


