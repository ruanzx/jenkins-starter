pipeline {
  agent none

  environment {
    CI = 'true'
    DB_SECRET = credentials('DB_SECRET');
  }

  triggers {
    pollSCM('* * * * *')
  }

  options {
    skipStagesAfterUnstable()

    // Only keep the 10 most recent builds
    buildDiscarder(logRotator(numToKeepStr:'10'))
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

      post {
        success {
          // Archive the built artifacts
          archive includes: 'dist/**/*'
        }
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
      agent {
        label 'agent-2'
      }
      when {
        expression {
          currentBuild.result == null || currentBuild.result == 'SUCCESS'
        }
      }
      steps {
        sh 'uname -a';
        println "deliver";
      }
    }

    stage('Deliver for development') {
      when {
        branch 'development'
      }
      steps {
        sh './jenkins/scripts/deliver-for-development.sh'
        input message: 'Finished using the web site? (Click "Proceed" to continue)'
        sh './jenkins/scripts/kill.sh'
      }
    }
    stage('Deploy for production') {
      when {
        branch 'production'
      }
      steps {
        sh './jenkins/scripts/deploy-for-production.sh'
        input message: 'Finished using the web site? (Click "Proceed" to continue)'
        sh './jenkins/scripts/kill.sh'
      }
    }
  }

  post {
    always {
      println "post always";
      archiveArtifacts artifacts: 'dist/**/*', fingerprint: true
      deleteDir() /* clean up our workspace */
    }
    success {
      echo 'I succeeeded!'
    }
    unstable {
      echo 'I am unstable :/'
    }
    failure {
      echo 'I failed :('
    }
    changed {
      echo 'Things were different before...'
    }
  }
}
