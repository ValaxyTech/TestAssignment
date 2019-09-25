pipeline {
    agent { label 'master' }
    /*environment {
          }*/
    stages {
        stage('Checking .NET core Version') {
            steps {
                sh '''
                    if [ ${BRANCH_NAME} = "release/1.0" ]
                    then
                        dotnet --version
                        echo $BRANCH_NAME
                        echo $BUILD_NUMBER
                        echo $BUILD_URL
                        curl ${BUILD_URL}/consoleText >> output.log
                        pwd
                    fi
                   '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                if [ ${BRANCH_NAME} = "release/1.0" ]
                then
                    cd HelloWorldSolution/
                    dotnet build -r win-x64 -o windowsbuild/
                    dotnet build -r linux-x64 -o ubuntubuild/
                fi
                   '''
            }
        }

        stage('Unit Tests') {
            steps {
                sh '''
                if [ ${BRANCH_NAME} = "release/1.0" ]
                then
                   dotnet test
                fi
                   '''
            }
        }
        
        stage('Triggering Integration Tests') {
            steps {
                sh '''
                   if [ ${BRANCH_NAME} = "master" ]
                   then
                        cd HelloWorldSolutions.Tests/
                        dotnet test HelloWorldSolutions.Integration.Tests.csproj
                   fi
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
