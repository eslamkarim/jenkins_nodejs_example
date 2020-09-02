def commit_id=''
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
                    def user_input = input(
                        id: 'commitId', message: 'Enter commit id to build:?', 
                        parameters: [
                        [$class: 'TextParameterDefinition', defaultValue: 'None', description: 'commit id to be built', name: 'commitId'],
                    ])
                    commit_id = user_input['commitId']
                    echo ("commit: "+user_input['commitId'])
                }
            }
        }
        stage("checkout commit"){
            steps{
                sh 'git checkout ${commit-id}'
            }
        }
        stage("show secrets"){
            steps{
                echo("host= "+ $HOST)
            }
        }

        stage("test docker"){
            steps{
                sh 'docker ps'
            }
        }

        
        stage("test kubectl"){
            steps{
                sh 'kubectl get po --namespace=dev'
            }
        }
    }
}