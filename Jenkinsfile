pipeline {
    tools{
        maven "Maven3"
    }
    agent any 
    environment {
            SCANNER_HOME= tool 'sonar-scanner'
            }
        stages {
            stage ("Checkout From Git") {
                steps {
                    git branch: 'main', url: 'https://github.com/bkrrajmali/springbootapp.git'
                }
            }
            stage ("Maven Build") {
                steps {
                    sh 'mvn clean install'
                }
            }
            stage ('Unit Test'){
                steps {
                    echo '<----------------------Unit Test Under Progess ------------------>'
                    sh 'mvn surefire-report:report'
                    echo '<----------------------Unit Test Done------------------>'
                }
            }
            stage ('Sonarqube analysis'){
            steps {
                script {
                    (credentialsId: 'sonar') {
                    sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }

        }
    }
