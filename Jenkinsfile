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
                sh 'dotnet restore Homies.sln'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build Homies.sln --configuration Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test Homies.sln --configuration Release --no-build --verbosity normal'
            }
        }
    }

    post {
        always {
            junit '**/TestResults/*.xml'
        }
        success {
            echo 'Build & Tests passed successfully!'
        }
        failure {
            echo 'Build or Tests failed!'
        }
    }
}