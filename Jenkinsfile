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
                }
            }
        }
        stage("deploy application"){
            steps{
                sh 'apk add gettext'
                sh "envsubst < configmap.yml > configmap.tmp && mv configmap.tmp configmap.yml"
                sh "cat configmap.yml"
                withDockerRegistry([ credentialsId: "nexus", url: "http://localhost:30654" ]){
                    withEnv(["ENV=$deploy_env"]) {
                        sh "envsubst < playbook.yaml > playbook.tmp && mv playbook.tmp playbook.yaml"
                        sh "ansible-playbook playbook.yaml"
                    
                    }
                }
            }
        }
    }
}