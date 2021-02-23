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
        /* stage('Quality Gate'){
            steps{
                sleep(5)
                timeout(time:1,unit:'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        } */
        stage('Deploy Backend'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'tomcat_login', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
        stage('API Test'){
            steps{
                dir('api-test'){
                    git credentialsId: 'github_login', url: 'https://github.com/leoamorimr/tasks-api-test'
                    bat 'mvn test'
                }
            }
        }
    }
}
