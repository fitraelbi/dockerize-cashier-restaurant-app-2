pipeline{
    agent any
    parameters {
        booleanParam(name: 'RUNTEST', defaultValue: true, description: 'Toggle this value for testing')
        choice(name: CICD, choices: ['CI', 'CICD'], description: 'Pick something')
    }
    stages{
        stage('Build Project'){
            steps{
                echo 'build....'
            }
        }
        stage('Run Testing'){
            steps{
                echo 'Testing....'
            }
        }
        stage('Deploy'){
            steps{
                echo 'DEploy....'
            }
        }
    }
}
