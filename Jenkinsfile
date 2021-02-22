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
        /* stage('Sonar analysis'){
            environment{
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps{
                withSonarQubeEnv('SONAR_LOCAL'){
                    bat "${scannerHome}/bin/sonar-scanner -e -D(parametros)"
                }
            }
        } */
    }
}