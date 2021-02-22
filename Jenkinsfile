pipeline{
    agent any
    stages{
        stage('Build Backend'){
            steps{
    //             execução de comando maven para gerar o war do projeto skipando os testes unitários.
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage('Unit Tests'){
            steps{
                bat 'mvn test'
            }
        }
    }
}