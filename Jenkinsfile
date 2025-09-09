pipeline {
    agent any

    environment {
        DOTNET_ROOT = "/usr/share/dotnet"
        CONFIGURATION = "Release"
        PUBLISH_DIR = "./publish"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/rmazolo2023/BasicJenkinsTest.git', branch: 'master'
            }
        }

        stage('Restore') {
            steps {
                echo '🔄 Restoring NuGet packages...'
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Building the project...'
                sh "dotnet build --configuration $CONFIGURATION"
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Running unit tests...'
                sh 'dotnet test --logger:"trx"'
            }
        }

        stage('Scan') {
            steps {
                echo '🔍 Running security scans...'
                // Example: sh 'dotnet sonarscanner begin ...'
                // Add integration with SonarQube or BlackDuck here
            }
        }

        stage('Package') {
            steps {
                echo '📦 Publishing build artifacts...'
                sh "dotnet publish -c $CONFIGURATION -o $PUBLISH_DIR"
                archiveArtifacts artifacts: "${PUBLISH_DIR}/**", fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying to development environment...'
                // Example: sh 'ansible-playbook deploy.yml'
                // Add deployment logic here
            }
        }
    }

    post {
        always {
            echo '📄 Archiving test results...'
            junit '**/TestResults/**/*.trx'
        }
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs for details.'
        }
    }
}
