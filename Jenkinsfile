pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
        disableConcurrentBuilds()
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore dependencies') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'dotnet restore'
                    } else {
                        bat 'dotnet restore'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'dotnet build --configuration Release --no-restore'
                    } else {
                        bat 'dotnet build --configuration Release --no-restore'
                    }
                }
            }
        }

        stage('Run all tests') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'dotnet test --configuration Release --no-build --verbosity normal'
                    } else {
                        bat 'dotnet test --configuration Release --no-build --verbosity normal'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build and all tests passed.'
        }
        failure {
            echo 'Build or tests failed. Check Console Output.'
        }
    }
}
