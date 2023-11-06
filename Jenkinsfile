pipeline {
    agent any
    tools {
  gradle 'adcb-gradle'
}

    stages {
        stage('Application build') {
            steps {
                    dir('/Users/chanchalsoni/Documents/adcb/demojava'){
                        sh 'gradle build -x test'
                    }
            }
        }
        stage('Docker image build') {
            steps {
                sh "docker build -t adcb-repo-ecr ."
                sh "docker tag adcb-repo-ecr:latest 160071257600.dkr.ecr.eu-north-1.amazonaws.com/adcb-repo-ecr:latest"

            }
        }
        stage('AWS ECR Login'){
            steps{
                sh 'aws ecr get-login-password --region eu-north-1 | docker login --username AWS --password-stdin 160071257600.dkr.ecr.eu-north-1.amazonaws.com'
            }
        }
        stage('Container Image Push'){
            steps{
                sh 'docker push 160071257600.dkr.ecr.eu-north-1.amazonaws.com/adcb-repo-ecr:latest'
            }
        }
        stage('Cloud deployment') {
            steps {
                container('kubectl') {
                    sh 'kubectl apply -f ./deploy/k8s.yaml'
                }
            }
        }
    }
}
