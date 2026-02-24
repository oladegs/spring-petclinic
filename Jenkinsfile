pipeline {
  agent any

  tools {
    jdk 'JDK17'
    maven 'Maven3'
  }

  triggers {
    cron('H/5 * * * 1')
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build') {
      steps { sh 'mvn clean test' }
    }

    stage('JaCoCo Report') {
      steps { sh 'mvn jacoco:report' }
    }

    stage('Package') {
      steps { sh 'mvn -DskipTests package' }
    }
  }
}