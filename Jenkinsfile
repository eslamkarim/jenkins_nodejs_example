ENV commit-id=''
pipeline {
   agent any
   stages {
       stage("get input from user"){
        steps {
            script {
            def user-input = input(
                id: 'userInput', message: 'Enter commit id to build:?', 
                parameters: [
                [$class: 'TextParameterDefinition', defaultValue: 'None', description: 'commit id to be built', name: 'commitId'],
            ])
            commit-id = user-input['commitId']
            echo ("commit: "+user-input['commitId'])
            }
        }
       }
       stage("checkout commit"){
           steps{
               sh 'git checkout ${commit-id}'
           }
       }
    }
}