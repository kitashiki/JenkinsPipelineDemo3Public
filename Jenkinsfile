pipeline {
    agent any
    parameters { // <------- 様々なタイプ(string, input, )のparametersがある
        string(name: 'PERSON', defaultValue: 'Jenkins', description: "What's your name?")
        
        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
    }
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
                
                // parameterのアクセスの仕方： ${params.PARAM_NAME}
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"
            }
        }
        stage('PowerShell') {
            steps {
                echo "PowerShell"
                // powershell "<Write-Host> ${env.BUILD_ID} "
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
     post {
        always {
            echo "Entire pipeline succeeded!"
            // mail to: banfuy@gmail.com, subject: 'The Pipeline succeeded! :('
        }
        failure { // <--------エラーの場合
            // mail to: banfuy@gmail.com, subject: 'The Pipeline failed :('
            echo "Entire pipeline failed..."
        }
    }
}   


