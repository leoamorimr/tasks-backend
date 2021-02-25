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
        stage('Deploy Frontend'){
            steps{
                dir('frontend'){
                    git credentialsId: 'github_login', url: 'https://github.com/leoamorimr/tasks-frontend'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'tomcat_login', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        }
        stage('Functional Test'){
            steps{
                dir('functional-test'){
                    git credentialsId: 'github_login', url: 'https://github.com/leoamorimr/tasks-functional-test'
                    bat 'mvn test'
                }
            }
        }
        stage('Deploy Prod'){
            steps{
                bat 'docker-compose build'
                bat 'docker-compose up -d'
            }
        }
        stage('Health Check'){
            steps{
                sleep(5)
                dir('functional-test'){
                    bat 'mvn verify -Dskip.surefire.tests'
                }
            }
        }
    }
    post{
        always{
            junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml, api-test/target/surefire-reports*.xml, functional-test/target/surefire-reports/*.xml, functional-test/target/failsafe-reports/*.xml'
        }
        unsuccessful{
            emailext attachLog: true, body: 'See the attached log below', subject: 'Build ${BUILD_NUMBER} has failed', to: 'leoamorimr+jenkins@gmail.com'
        }
        fixed{
            emailext attachLog: true, body: 'See the attached log below', subject: 'Build ${BUILD_NUMBER} is fine!G', to: 'leoamorimr+jenkins@gmail.com'
        }
    }
}
