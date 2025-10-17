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
        stage('DayTimeStamp Test') {
            steps {
                echo "DayTimeStamp!"
                echo "${BUILD_TIMESTAMP}"
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
        stage('Build0') {
            steps {
                sh'''
                    # Jenkins env BRANCH_NAME will be available for multi-branch pipeline. Ref: https://www.jenkins.io/doc/book/pipeline/multibranch/#additional-environment-variables
                    APP_VERSION=${BRANCH_NAME}-0.1.${BUILD_NUMBER}
                    
                    HELM_CHART_FILE_NAME=app-staging-${APP_VERSION}.tgz

                    mkdir -p outputs

                    echo ${HELM_CHART_FILE_NAME} > outputs/staging-chart-filename.txt && cat outputs/staging-chart-filename.txt
                    
                    ls -l outputs/
                '''
                // フォルダーをstashすることで、次のステージにフォルダーを持ち込める
                stash name: "chart_file_name", includes: "outputs/*"
            }
        }
        stage('Test0') {
            steps {
                unstash "chart_file_name" // <------- unstash
                sh 'pwd && ls -l outputs/'
                
                sh '''
                    CHART_FILE_NAME=$(cat outputs/staging-chart-filename.txt)
                    echo "chart file name is ${CHART_FILE_NAME}"
                '''
            }
        }
        stage(" sh()") {
            steps {
                // short form 
                sh 'echo hello'

                // long form
                sh([script: 'echo hello'])
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
        success {
            echo "All stage succeeded!"
            // mail to: banfuy@gmail.com, subject: 'All stage succeeded! :('
        }
        failure { // <--------エラーの場合
            // mail to: banfuy@gmail.com, subject: 'The Pipeline failed :('
            echo "Entire pipeline failed..."
        }
    }
}   


