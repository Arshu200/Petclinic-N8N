pipeline {
  agent any

  tools {
    maven 'Maven'
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/Arshu200/Petclinic-N8N.git'
      }
    }
    stage('Build') {
      steps {
        sh 'mvm clean package'
      }
    }
    stage('Test') {
      steps {
        sh 'mvm test'
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deployment step - replace with actual deployment commands'
      }
    }
  }

  post {
    failure {
      script {
        try {
          echo "Collecting pipeline logs..."

          def log = currentBuild.rawBuild.getLog(1000)
          def logText = log.join("\n")

          def payload = groovy.json.JsonOutput.toJson([log: logText])
          def response = httpRequest(
            httpMode: 'POST',
            url: 'http://54.80.157.14:5678/webhook/d82dd1b1-1a68-423e-9ec1-f7c1894ce73b',
            contentType: 'APPLICATION_JSON',
            requestBody: payload
          )

          echo "Webhook response: ${response.status} - ${response.content}"
        } catch (Exception e) {
          echo "ERROR sending webhook: ${e.message}"
        }
      }
    }
  }
}
