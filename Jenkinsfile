pipeline {  
    agent {
        label 'docker'
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    environment {
        HEROKU_API_KEY = credentials('heroku-api-key') // Github
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials') // Docker Hub
        IMAGE_NAME = 'ivanaki/jenkins-example-symfony'
        IMAGE_TAG = 'latest'
        APP_NAME = 'jenkins-example-symfony'
        KUBECONFIG_CREDENTIAL = credentials('kuber')
    }
    stages {
        stage('Build') {
            steps {
                sh 'echo $PATH'
                sh 'whoami'
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }
        stage('Test') {
            steps {
                script {
                    // Run PHPUnit with code coverage
                    sh 'docker run -e XDEBUG_MODE=coverage $IMAGE_NAME:$IMAGE_TAG php vendor/bin/phpunit --coverage-html=coverage tests'

                    // Archive the coverage report as a build artifact
                    archiveArtifacts artifacts: 'tests/coverage/**/*', allowEmptyArchive: true
                }
            }
        } 
    }
}
