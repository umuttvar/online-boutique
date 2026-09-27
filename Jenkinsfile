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
                    sh 'sonar-scanner -Dsonar.token=$SONAR_AUTH_TOKEN'
            }
            }
        }
        
        stage ('Trivy Security Scan') {
            steps{
                sh '''
                docker run --rm aquasec/trivy image \
                --severity CRITICAL,HIGH \
                 us-central1-docker.pkg.dev/online-boutique-ci/microservices-demo/adservice:v0.10.7
                 '''
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