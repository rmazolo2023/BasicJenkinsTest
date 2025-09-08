pipeline {
    agent any

    environment {
        DOTNET_ROOT = "/usr/share/dotnet"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/rmazolo2023/BasicJenkinsTest.git', branch: 'master'
            }
        }

        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test --logger:"trx"'
            }
        }

        stage('Scan') {
            steps {
                echo 'Running security scans...'
                // Integrate SonarQube or BlackDuck here
            }
        }

        stage('Package') {
            steps {
                sh 'dotnet publish -c Release -o ./publish'
                archiveArtifacts artifacts: 'publish/**', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to development environment...'
                // Use Ansible or Helm here
            }
        }
    }

    post {
        always {
            junit '**/*.trx'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
