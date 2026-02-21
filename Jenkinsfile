pipeline {
    agent any

    triggers {
        githubPush()
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timeout(time: 30, unit: 'MINUTES')
    }

    stages {
        stage('Checkout') {
            steps {
                echo '🔄 Checking out code...'
                checkout scm
            }
        }

        stage('Restore Dependencies') {
            steps {
                echo '📦 Restoring NuGet packages...'
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Building..'
                sh 'dotnet build --configuration Release --no-restore'
            }
        }

        stage('Run Tests') {
            steps {
                echo '🧪 Running all tests...'
                sh 'dotnet test --configuration Release --no-build --verbosity normal'
            }
        }

        stage('Publish Results') {
            steps {
                echo '📊 Publishing test results...'
                junit '**/TestResults/*.trx'
            }
        }
    }

    post {
        always {
            echo '✅ Pipeline execution completed'
            cleanWs()
        }
        success {
            echo '✅ Build and tests passed!'
        }
        failure {
            echo '❌ Build or tests failed!'
        }
    }
}
