pipeline{
    agent any
    parameters {
        booleanParam(name: 'RUNTEST', defaultValue: true, description: 'Toggle this value for testing')
        choice(name: 'DEV/PRODUCTION', choices: ['DEVELOP', 'PRODUCTION'], description: 'Choose Server')
    }
    environtment {
        registryCredential = 'dockerHub'
    }
    stages{
        stage('Build Project'){
            steps{
                nodejs('node12') {
                    sh 'yarn install'
                }
            }
        }
        stage('Build Docker Image'){
            steps{
               script {
                commitHash = sh (script : "git log -n 1 --pretty=format:'%H'", returnStdout: true)
                docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                    def dockerfile = 'dockerfile'
                    def customImage = docker.build("frontend:${env.BUILD_ID}", "-f ${dockerfile} ./frontend")
                    customImage.push()
                }
               }
            }
        }
        stage('Remove Image'){
            steps{
                echo 'Remove....'
            }
        }
        stage('Run Testing'){
            when {
                expression {
                    params.RUNTEST
                }
            }
            steps{
                echo 'Testing....'
            }
        }
        stage('Deploy'){
            steps{
                echo 'Deploy....'
            }
        }
    }
}
