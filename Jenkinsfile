pipeline {
  agent none

  environment {
    CI = 'true'
    DB_SECRET = credentials('DB_SECRET');
  }

  tools {
    git 'localGit'
    jdk 'localJava'
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

      }
    }

    stage('Deliver') {
      steps {

      }
    }
  }

  post {
    always {

    }
  }
}
