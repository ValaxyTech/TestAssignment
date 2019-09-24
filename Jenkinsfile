pipeline {
    agent { label 'master' }
    stages {
        stage('Checking .NET core Version') {
            steps {
                sh 'dotnet --version'
            }
        }
        stage('Build') {
            steps {
                sh '''
                    cd HelloWorldSolution/
                    dotnet build -r win-x64 -o windowsbuild/
                    dotnet build -r linux-x64 -o ubuntubuild/
                   '''
            }
        }

        stage('Unit Tests') {
            steps {
                sh '''
                   pwd
                   '''
            }
        }
    }
}
