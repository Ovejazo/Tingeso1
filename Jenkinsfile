pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages{
         stage('Build JAR File'){
                steps{
                    // Primero hacemos el checkout
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Ovejazo/Tingeso1']])
                    
                    // Mostramos el contenido del directorio raíz
                    bat 'echo "==== Contenido del directorio raiz ====" && dir'
                    
                    // Intentamos mostrar el contenido del directorio payroll-backend
                    bat 'echo "==== Contenido del directorio payroll-backend (si existe) ====" && dir payroll-backend || echo "El directorio payroll-backend no existe"'
                    
                    // Procedemos con Maven solo si el directorio existe
                    script {
                        if (fileExists('payroll-backend')) {
                            dir("payroll-backend"){
                                bat 'echo "==== Ejecutando Maven en payroll-backend ====" && dir'
                                bat "mvn clean install"
                            }
                        } else {
                            error "El directorio payroll-backend no existe después del checkout"
                        }
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