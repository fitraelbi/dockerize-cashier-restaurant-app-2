pipeline{
    agent any
    parameters {
        booleanParam(name: 'RUNTEST', defaultValue: true, description: 'Toggle this value for testing')
        choice(name: CICD, choices: ['CI', 'CICD'], description: 'Pick something')
    }
    stages{
        stage('Build Project'){
            step{
                echo 'build....'
            }
        }
        stage('Run Testing'){
            step{
                echo 'Testing....'
            }
        }
        stage('Deploy'){
            step{
                echo 'DEploy....'
            }
        }
    }
}