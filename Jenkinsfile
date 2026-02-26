pipeline {
  agent any
  options { timestamps() }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build & Test') {
      steps {
        bat 'mvn -v'
        // Ensure Cucumber JSON is generated; for Cucumber 6.x, use the plugin property below:
        bat 'mvn -f pom.xml clean test -DskipTests=false -Dcucumber.plugin="json:target/cucumber.json"'
      }
    }

    stage('Publish Cucumber Report') {
      steps {
        // Requires "Cucumber Reports" Jenkins plugin
        cucumber fileIncludePattern: 'target/cucumber.json'
      }
    }
  }

  post {
    always {
      junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
      archiveArtifacts artifacts: 'target/**/*.html, target/**/*.json, target/**/*.xml', onlyIfSuccessful: false
    }
  }
}
