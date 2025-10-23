pipeline {
    agent {
        label 'built-in'
    }
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
                // BUILD TIMESTAMP をプラグインしたので、時間の環境変数が使えるようになった。表示方式はJenkinsのsystemで設定できる。今はyyyyMMddにしている。
                echo "${BUILD_TIMESTAMP}"
                echo "${env.BUILD_TIMESTAMP}"
                sh 'date'
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
        stage('TestStep') {
            steps {
               echo "Node_Name → ${NODE_NAME} "
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

                    mkdir -p outputs${BUILD_NUMBER}/${BUILD_NUMBER}

                    echo ${HELM_CHART_FILE_NAME} > outputs${BUILD_NUMBER}/staging-chart-filename.txt && cat outputs${BUILD_NUMBER}/staging-chart-filename.txt
                    
                    ls -l outputs${BUILD_NUMBER}/
                '''
                // フォルダーをstashすることで、次のステージにフォルダーを持ち込める
                stash name: "chart_file_name", includes: "outputs${BUILD_NUMBER}/*"
            }
        }
        stage('Test0') {
            steps {
                unstash "chart_file_name" // <------- unstash
                sh 'pwd && ls -l outputs${BUILD_NUMBER}/'
                
                sh '''
                    CHART_FILE_NAME=$(cat outputs${BUILD_NUMBER}/staging-chart-filename.txt)
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
            archiveArtifacts artifacts: "Jenkinsfile",
                fingerprint: true,
                allowEmptyArchive: true
            archiveArtifacts artifacts: "outputs${BUILD_NUMBER}/**/*",
                fingerprint: true,
                allowEmptyArchive: true
            // archiveArtifacts artifacts: "outputs/*",
            //     fingerprint: true,
            //     allowEmptyArchive: true
            archiveArtifacts "outputs/**/*"
            //Groovy 実験 プラグイン後
            script {
                def i = 2
                echo "script adding"
                echo "i は ${i}"
                println i
                sh "mkdir -p OUTputs${BUILD_NUMBER}/${BUILD_NUMBER}"
                sh "cp Jenkinsfile OUTputs${BUILD_NUMBER}/${BUILD_NUMBER}/JenkinsfileCOPY"
            }
            archiveArtifacts artifacts: "OUTputs${BUILD_NUMBER}/**/*",
                fingerprint: true,
                allowEmptyArchive: true
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