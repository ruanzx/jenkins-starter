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
          label 'agent-2' // Require a build executor with docker
        }
      }
      steps {
        // sh '''#!/bin/bash
				// 	echo "JAVA_HOME = ${JAVA_HOME}";
				// 	echo "PATH = ${PATH}";
        //   echo env;
				// 	echo "this is the project id environment";
				// '''
        sh 'uname -a';
        println "Init success..";
      }
    }

    stage('Test') {
      // steps {
      //   sh 'echo "test"';
      // }

      parallel {
        stage('Unit Test') {
          steps {
            println "Execute Unit test";
          }
        }
        stage('Integration Test') {
          // when { expression { return isTimeTriggeredBuild() } }
          steps {
            println "Execute Integration Test";
          }
        }
      }
    }

    stage('Deliver') {
      steps {
        sh 'uname -a';
        println "deliver";
      }
    }
  }

  post {
    always {
      println "post always";
    }
  }
}
