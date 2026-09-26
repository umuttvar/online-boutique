pipeline {
    agent any
    stages{
        stage("SCM"){
            steps{
                checkout scm
            }
        }

        stage ('Helm Lint') {
            steps{
                sh 'helm lint online-boutique-chart'
            }
        }
        
        stage ('SonarQube Analysis'){
            steps{
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner'
            }
            }
        }
    }

    post{
        success{
            echo 'Tum kontroller basarili'
        }
        failure{
             echo 'Bir hata olustu, loglara bak'
        }
    }
}