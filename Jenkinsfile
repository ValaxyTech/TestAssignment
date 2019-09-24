pipeline {
    agent { label 'master' }
    environment {
        BRANCH_NAME = GIT_BRANCH.replaceAll('[/,\\.]','_')
          }
    stages {
        stage('Checking .NET core Version') {
            steps {
                sh '''
                    dotnet --version
                    echo $BRANCH_NAME
                   '''
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
                   dotnet test
                   '''
            }
        }
       /*stage('Linting with SonarQube') {
            def sqScannerMsBuildHome = tool 'Scanner for MSBuild 4.6'
            withSonarQubeEnv('SonarQube Server') {
            bat "${sqScannerMsBuildHome}\\SonarQube.Scanner.MSBuild.exe begin /k:myKey"
            bat 'MSBuild.exe /t:Rebuild'
            bat "${sqScannerMsBuildHome}\\SonarQube.Scanner.MSBuild.exe end"
    }
  }*/
    }
}
