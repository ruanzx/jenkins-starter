pipeline {
  agent none

  environment {
    CI = 'true'
    DB_SECRET = credentials('DB_SECRET');
  }

  stages {
    stage('Build') {
      agent {
        docker {
          image 'node:12.13.0-alpine'
          args '-p 3000:3000'
        }
      }
      steps {
        sh '''#!/bin/bash
					echo "JAVA_HOME = ${JAVA_HOME}";
					echo "PATH = ${PATH}";
          echo env;
					echo "this is the project id environment";
				'''
        sh 'echo "npm install"';
        println "Init success..";
      }
    }

    stage('Test') {
      steps {
        sh 'echo "test"';
      }
    }

    stage('Deliver') {
      steps {
        sh 'echo "deliver"';
      }
    }
  }

  post {
    always {
      sh 'echo "post always"';
    }
  }
}
