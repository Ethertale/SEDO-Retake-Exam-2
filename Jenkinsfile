pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    options {
        timeout(time: 30, unit: 'MINUTES')
    }

    environment {
        DOTNET_ROOT = "/usr/share/dotnet"
        PATH = "${DOTNET_ROOT}:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/Ethertale/SEDO-Retake-Exam-2.git']]
                ])
            }
        }

        stage('Restore') {
            steps {
                bat 'dotnet restore Homies.sln'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build Homies.sln --configuration Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                bat 'dotnet test Homies.sln --configuration Release --no-build --verbosity normal'
            }
        }
    }
}