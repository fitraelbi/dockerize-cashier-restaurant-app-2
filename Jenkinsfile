pipeline{
    agent any
    parameters {
        booleanParam(name: 'RUNTEST', defaultValue: true, description: 'Toggle this value for testing')
        choice(name: 'DEV/PRODUCTION', choices: ['DEVELOP', 'PRODUCTION'], description: 'Choose Server')
    }
    environment {
        registry = "fitrakz/frontend"
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
                 def dockerfile = 'dockerfile'
                docker.withRegistry('', registryCredential) {
                    def app = docker.build(, "-f ${dockerfile} https://github.com/fitraelbi/cashier-restaurant-app-vue.git")
                    app.push("latest")
                  }
               }
            }
        }
        stage('Remove Image'){
            steps{
                echo 'Remove....'
                sh "docker rmi $registry:latest"
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
