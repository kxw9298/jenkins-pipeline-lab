pipeline {
  agent none
  stages {
    stage('Fluffy Build') {
      parallel {
        stage('Build Java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          post {
            success {
              stash(name: 'Java 8', includes: 'target/**')
            }

          }
          steps {
            sh './jenkins/build.sh'
          }
        }

        stage('Build Java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          post {
            success {
              archiveArtifacts 'target/*.jar'
              stash(name: 'Java 7', includes: 'target/**')
            }

          }
          steps {
            sh './jenkins/build.sh'
          }
        }

      }
    }

    stage('Fluffy Test') {
      parallel {
        stage('Backend Java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          post {
            always {
              junit 'target/surefire-reports/**/TEST*.xml'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-backend.sh'
          }
        }

        stage('Frontend') {
          agent {
            node {
              label 'java8'
            }

          }
          post {
            always {
              junit 'target/test-results/**/TEST*.xml'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-frontend.sh'
          }
        }

        stage('Performance Java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-performance.sh'
          }
        }

        stage('Static Java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-static.sh'
          }
        }

        stage('Backend Java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          post {
            always {
              junit 'target/surefire-reports/**/TEST*.xml'
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/test-backend.sh'
          }
        }

        stage('Frontend Java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          post {
            always {
              junit 'target/test-results/**/TEST*.xml'
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/test-frontend.sh'
          }
        }

        stage('Performance Java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/test-performance.sh'
          }
        }

        stage('Static Java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/test-static.sh'
          }
        }

      }
    }

    stage('Confirm Deploy') {
      when {
        branch 'master'
      }
      steps {
        input(message: 'Okay to Deploy to Staging?', ok: 'Let\'s Do it!')
      }
    }

    stage('Fluffy Deploy') {
      agent {
        node {
          label 'java7'
        }

      }
      when {
        branch 'master'
      }
      steps {
        unstash 'Java 7'
        sh "./jenkins/deploy.sh ${params.DEPLOY_TO}"
      }
    }

  }
  parameters {
    string(name: 'DEPLOY_TO', defaultValue: 'dev', description: '')
  }
}