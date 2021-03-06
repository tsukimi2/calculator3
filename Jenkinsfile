pipeline {
  agent { label 'java-docker-slave' }
  stages {
    stage("Compile") {
      steps {
        sh "./gradlew compileJava"
      }
    }
    stage("Unit test") {
      steps {
        sh "./gradlew test"
      }
    }
    stage("Code coverage") {
      steps {
        sh "./gradlew jacocoTestReport"
      }
    }
    stage("Static code analysis") {
      steps {
        sh "./gradlew checkstyleMain"
      }
    }
    stage("Package") {
      steps {
        sh "./gradlew build"
      }
    }
    stage("Docker build") {
      steps {
        sh "docker build -t osiris65/calculator:${BUILD_TIMESTAMP} ."
      }
    }
    stage("Docker login") {
      steps {
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhubCredential', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
          sh "docker login --username $USERNAME --password $PASSWORD"
        }
      }
    }
    stage("Docker push") {
      steps {
        sh "docker push osiris65/calculator:${BUILD_TIMESTAMP}"
      }
    }
  }
}
