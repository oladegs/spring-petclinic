pipeline {
  agent any

  triggers {
    cron('H/5 * * * 1') // every 5 minutes on Mondays
  }

  options {
    timestamps()
    disableConcurrentBuilds()
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build & Test') {
      steps {
        bat '.\\mvnw.cmd -B clean test'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    stage('JaCoCo Coverage') {
      steps {
        bat '.\\mvnw.cmd -B jacoco:report'
      }
      post {
        always {
          archiveArtifacts artifacts: 'target/site/jacoco/**', fingerprint: true, allowEmptyArchive: true
        }
      }
    }

    stage('Package Artifact') {
      steps {
        bat '.\\mvnw.cmd -B -DskipTests package'
      }
      post {
        success {
          archiveArtifacts artifacts: 'target/*.jar', fingerprint: true, allowEmptyArchive: true
        }
      }
    }
  }
}