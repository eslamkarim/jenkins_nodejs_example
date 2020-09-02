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
                    def userInput = input(
                        id: 'userInput', message: 'Enter commit id to build:', 
                        parameters: [
                        string(defaultValue: 'None', description: 'commit id to be built', name: 'commitId')
                    ])
                    commit_id = userInput.commitId
                    echo ("commit: ${commit_id}")
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