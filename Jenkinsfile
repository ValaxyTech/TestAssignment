pipeline {
    agent { label 'master' }
    /*environment {
        BRANCH_NAME = GIT_BRANCH
          }*/
    stages {
        stage('Checking .NET core Version') {
            if (env.BRANCH_NAME == 'release/1.0') {
             sh 'dotnet --version'
        } else {
            Exit 1
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
