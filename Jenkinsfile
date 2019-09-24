pipeline {
    agent { label 'master' }
    stages {
        stage('Checking .NET core Version') {
            steps {
                sh 'dotnet --version'
            }
        }
    }
}
