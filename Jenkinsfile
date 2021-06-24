pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Build demo-app'
        sh 'sh run_build_script.sh'
      }
    }

    stage('Linux tests') {
      parallel {
        stage('Linux tests') {
          steps {
            echo 'Run Linux tests'
            sh 'sh run_linux_tests.sh'
          }
        }

        stage('error') {
          steps {
            echo 'Run windows tests'
          }
        }

      }
    }

    stage('Deploy staging') {
      steps {
        echo 'Deploy to staging environment'
        input 'Ok to deploy to Prod'
      }
    }

    stage('Deploy production') {
      steps {
        echo 'Deploy to Prod'
      }
    }

  }
  post {
    always {
      archiveArtifacts(artifacts: 'target/demoapp.jar', fingerprint: true)
    }

    failure {
      mail(to: 'ci-team@example.com', subject: "Failed Pipeline ${currentBuild.fullDisplayName}", body: " For details about the failure, see ${env.BUILD_URL}")
    }

  }
}