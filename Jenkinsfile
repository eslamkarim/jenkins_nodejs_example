def deploy_env=''
pipeline {
    agent any
    environment {
        HOST     = credentials('db-host')
        USERNAME = credentials('db-username')
        PASSWORD = credentials('db-password')
        DATABASE = credentials('db-database')
    }
    stages {
        stage("get input from user"){
            steps {
                script {
                    def userInput = input(
                        id: 'userInput', message: 'Choose enviroment to deploy on:', 
                        parameters: [
                        choice(name: 'deployEnviroment', choices: ['dev','test'])
                    ])
                    deploy_env = userInput
                    echo ("commit: ${deploy_env}")
                }
            }
        }
        stage("deploy application"){
            steps{
                sh 'apk add gettext'
                sh "envsubst < configmap.yml | kubectl apply -f . -n $deploy_env"
            }
        }
    }
}