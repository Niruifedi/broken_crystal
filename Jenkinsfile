node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'broken_crystal';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
  post {
        always {
            echo 'Slack Notifications'
            script {
                def color = COLOR_MAP[currentBuild.currentResult] ?: 'warning' // Default to 'warning' if build result is not FAILURE or SUCCESS
                slackSend (
                    color: color,
                    channel: '#jenkins-build'
                    message: "${currentBuild.currentResult} Job ${env.JOB_NAME}\nbuild ${env.BUILD_NUMBER}\nMore info at: ${env.BUILD_URL}"
                )
            }
        }
  }
}
