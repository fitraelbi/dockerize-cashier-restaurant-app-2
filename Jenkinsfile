pipeline{
    agent any
    parameters {
        booleanParam(name: 'RUNTEST', defaultValue: true, description: 'Toggle this value for testing')
        choice(name: 'DEV/PRODUCTION', choices: ['DEVELOP', 'PRODUCTION'], description: 'Choose Server')
    }
    environment {
        registry = "fitrakz/frontend"
        registry_backend = "fitrakz/backend"
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
                    def app = docker.build(registry, "-f ${dockerfile} https://github.com/fitraelbi/cashier-restaurant-app-vue.git")
                    app.push("latest")
                    def backend = docker.build(registry_backend, "-f ${dockerfile} https://github.com/fitraelbi/cashier-restaurant-app-nodejs3.git#main")
                    backend.push("latest")
                  }
               }
            }
        }
        stage('Remove Image'){
            steps{
                echo 'Remove....'
                sh "docker rmi ${registry}:latest"
                sh "docker rmi ${registry_backend}:latest"
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
                script {
                   sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'Production',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'docker pull fitrakz/frontend:latest;  docker pull fitrakz/backend:latest; docker-compose down; docker-compose rm -f; docker-compose pull; docker-compose up --build -d;',
                                        execTimeout: 120000,
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}
