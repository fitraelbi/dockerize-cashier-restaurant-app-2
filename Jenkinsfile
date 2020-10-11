pipeline{
    agent any
    parameters {
        booleanParam(name: 'RUNTEST', defaultValue: true, description: 'Toggle this value for testing')
        choice(name: 'DEV/PRODUCTION', choices: ['DEVELOP', 'PRODUCTION'], description: 'Choose Server')
    }
    environment {
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
                docker.withRegistry('', registryCredential) {
                    def dockerfile = 'dockerfile'
                    def dockerpath = "./frontend2"
                    def customImage = docker.build("frontend:latest", "-f ${dockerfile} https://github.com/fitraelbi/cashier-restaurant-app-vue.git")
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
