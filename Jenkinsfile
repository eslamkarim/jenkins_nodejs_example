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
                    commit_id = userInput
                    echo ("commit: ${commit_id}")
                }
            }
        }
        stage("checkout commit"){
            steps{
                sh 'git checkout ${commit_id}'
            }
        }
        stage("build docker image"){
            steps{
                sh 'docker build -t localhost:30654/voda-node:latest .'
            }
        }
        stage("push docker image"){
            steps{
                withDockerRegistry([ credentialsId: "nexus", url: "http://localhost:30654" ]) {
                    sh 'docker push localhost:30654/voda-node:latest'
                }
            }
        }
    }
}