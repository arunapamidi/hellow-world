node{
  stage('SCM Checkout'){
  git 'https://github.com/arunapamidi/hellow-world'
  }
  stage('compile-package'){
    def mvnHome = tool name: 'maven-3',type: 'maven'
    sh "${mvnHome}/bin/mvn package"
  }
  stage('Sonarqube Analysis'){
    def mvnHome = tool name: 'maven-3',type: 'maven'
    with SonarQubeEnv('sonar-6'){
      sh "${mvnHome}/bin/mvn sonar:sonar"
    }
  }
  stage('quality gate status check'){
    timeout(time: 1, unit:hours) {
      def qg =waitForQualityGate()
      if (qg.status != 'OK'){
        slackSend baseUrl: 'https://hooks.slack.com/services/',
       channel: '#jenkins-pipeline-demo',
       color: 'danger', 
       message: 'Sonarqube analysis failed', 
       teamDomain: 'javahomecloud',
       tokenCredentialId: 'slack-demo'
        error "Pipeline aborted due to quality gate failure: ${qg.status}"
      }
    }
  }
  stage('deploy to tomcat'){
    sshagent(tomcat-dev){
      sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.29.242:/opt/tomcat8/webapps/'
    }
  }
  stage('Email Notification'){
  }
  stage('Slack Notification'){
       slackSend baseUrl: 'https://hooks.slack.com/services/',
       channel: '#jenkins-pipeline-demo',
       color: 'good', 
       message: 'Welcome to Jenkins, Slack!', 
       teamDomain: 'javahomecloud',
       tokenCredentialId: 'slack-demo'
   }
}
