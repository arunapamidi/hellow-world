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
    with SonarQubeEnv('Sonar'){
      sh "${mvnHome}/bin/mvn sonar:sonar"
    }
  }
  stage('Email Notification'){
  }
}
