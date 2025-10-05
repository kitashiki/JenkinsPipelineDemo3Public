pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello from GitHub hook trigger'
                sh'echo "Hello again!"'
                sh """
                  echo "sh again!"
                  echo 'sh again again!!'
                """
            }
        }
        stage('Example') {
            steps {
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            }
        }
        stage('PowerShell') {
            steps {
                echo "PowerShell"
                powershell "<Write-Host> ${env.BUILD_ID} "
                echo "PowerShell2"
            }
        }
        stage('Jenkins Envs') {
            steps {
                echo "RunningAgain ${env.BUILD_ID} on ${env.JENKINS_URL}"

                sh '''
                  echo "Running ${BUILD_NUMBER} and ${BUILD_ID} for ${JOB_NAME} on node ${NODE_NAME} at URL:${JENKINS_URL}"
                '''
            }
        }
        stage('Build') {
            steps {
                echo 'Building'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying' 
                }
            }
         stage('Test') {
            steps {
                echo 'Testing'
            }
        }
        stage('Release') {
            steps {
                echo 'Releasing'
            }
        } 
    }
}   


