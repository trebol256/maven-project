pipeline {
  agent none
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          agent {
            node {
              label 'Ubuntu'
            }

          }
          steps {
            sh 'mvn clean package'
            archiveArtifacts '**/*.war'
          }
        }
        stage('Analysis') {
          steps {
            sh 'mvn checkstyle:checkstyle'
            checkstyle()
          }
        }
      }
    }
    stage('Aprove') {
      agent {
        node {
          label 'Windows'
        }

      }
      steps {
        input(message: 'desea desplegar?', ok: 'SI')
      }
    }
    stage('despliegue') {
      agent {
        node {
          label 'Windows'
        }

      }
      steps {
        copyArtifacts(projectName: 'maven-project', fingerprintArtifacts: true, flatten: true)
      }
    }
  }
  environment {
    TOMCAT_HOME = 'C:\\Tomcat_8.5\\webapps'
  }
}