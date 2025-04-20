pipeline {
    agent any
    tools{
        maven "maven"
    }
    stages{
        stage('Build JAR File'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Ovejazo/Tingeso1']])
                dir("BackTingeso"){
                    bat 'mvn clean install'
                }
            }
        }

        stage('Unit Tests') {
            steps {
                // Run Maven 'test' phase. It compiles the test sources and runs the unit tests
                bat 'mvn test' // Use 'bat' for Windows agents or 'sh' for Unix/Linux agents
            }
        }

        stage('Build docker image'){
            steps{
                script{
                    bat 'docker build -t mtisw/book_service:latest .'
                }
            }
        }
        stage('Push image to Docker Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dhpswid', variable: 'dhpsw')]) {
                        bat 'docker login -u mtisw -p %dhpsw%'
                   }
                   bat 'docker push mtisw/book_service:latest'
                }
            }
        }
    }
}