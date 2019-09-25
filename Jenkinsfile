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

  stage('Archiving Builds and build outputs') {
            steps {
                sh '''
                   pwd
                   if [[ ! -e ArchiveBuilds/ ]]; then
                        mkdir 
                   fi ArchiveBuilds/
                   cd ArchiveBuilds/
                   curl -u jenkins:jenkins http://localhost:8080/job/LiveOptics/job/release%252F1.0/54/consoleText >> build${BUILD_NUMBER}.log
                   cd ..
                   cd HelloWorldSolution/
                   zip windowsbuild.zip windowsbuild/
                   mv windowsbuild.zip windowsbuild${BUILD_NUMBER}.zip
                   mv windowsbuild${BUILD_NUMBER}.zip ../ArchiveBuilds
                   cd ArchiveBuilds/
                   zip ../HelloWorldSolution/windowsbuild${BUILD_NUMBER}.zip ../build${BUILD_NUMBER}.log
                   '''
            }
        }
    }
}
