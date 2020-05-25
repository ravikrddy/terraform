pipeline {
  environment {
    ENV_NAME = "${ENV_NAME}"
    APP_BUILD_NUMBER = "${APP_BUILD_NUMBER}"
  }

  stages {
    stage('Build') {
      agent any
      steps{
        script {
            if (env.ENV_NAME == 'dev') {
                sh "echo $BUILD_NUMBER"
            }
            else {
                sh "echo $APP_BUILD_NUMBER; echo $ENV_NAME"
            }

        }
      }
    }
    stage('trigger-ppe-pipeline') {
      agent any
      when {
        environment name: 'ENV_NAME', value: 'dev'
      }
      steps {
        build (
            job: 'ppe-pipeline-plm',
            parameters: [
                [
                    $class: 'StringParameterValue',
                    name: 'APP_BUILD_NUMBER',
                    value: "${BUILD_NUMBER}",
                ]
            ]
        )
      }
    }
    stage('trigger-prod-pipeline') {
      agent any
      when {
        environment name: 'ENV_NAME', value: 'ppe'
      }
      steps {
        build (
            job: 'prod-pipeline-plm',
            parameters: [
                [
                    $class: 'StringParameterValue',
                    name: 'APP_BUILD_NUMBER',
                    value: "${APP_BUILD_NUMBER}",
                ]
            ]
        )
      }
    }
  }
}
