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

  stage ('Create build output'){
        steps{
        // Make the output directory.
        sh "mkdir -p output"

        // Write an useful file, which is needed to be archived.
        writeFile file: "output/usefulfile.txt", text: "This file is useful, need to archive it."

        // Write an useless file, which is not needed to be archived.
        writeFile file: "output/uselessfile.md", text: "This file is useless, no need to archive it."

        stage "Archive build output"
        
        // Archive the build output artifacts.
        archiveArtifacts artifacts: 'output/*.txt', excludes: 'output/*.md'
        }
    }
  }
}
