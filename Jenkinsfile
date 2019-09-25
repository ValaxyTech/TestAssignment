//#######################################################################################//
//Jenkinsfile for Building , testing, integration test, sonar linting and archiving builds.
//#######################################################################################//
pipeline {

    //#################################################################//
    //This jenkins file should be run on jenkins ubuntu agent as rquired.
    //#################################################################//
    agent { label 'master' }
    stages {

        //#########################//
        //Checking .NETCORE version.
        //#########################//
        stage('Checking .NET core Version') {
            steps {
                sh '''
                        dotnet --version
                        echo $BRANCH_NAME
                        echo $BUILD_NUMBER
                        echo $BUILD_URL
                   '''
            }
        }

        //################//
        //Building the code.
        //################//
        stage('Build') {
            steps {
                sh '''
                if [ ${BRANCH_NAME} = "release/1.0" ]
                then
                    cd HelloWorldSolution/
                    dotnet build -r linux-x64 -o ubuntubuild/
                fi
                   '''
            }
        }

        //############//
        //Unit Testing.
        //###########//
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
        
        //###################//
        //Integration Testing.
        //###################//
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

        //#############################################################//
        //Scanning and linting with SonarQube.
        //Note: SonarQube container sholud be a windows box in this case.
        //#############################################################//
        /*stage('Linting with SonarQube') {
                def sqScannerMsBuildHome = tool 'Scanner for MSBuild 4.6'
                withSonarQubeEnv('SonarQube Server') {
                bat "${sqScannerMsBuildHome}\\SonarQube.Scanner.MSBuild.exe begin /k:myKey"
                bat 'MSBuild.exe /t:Rebuild'
                bat "${sqScannerMsBuildHome}\\SonarQube.Scanner.MSBuild.exe end"
        }
    }*/
  //########################################################################################//
  // "Archived Version of Build artifacts and Build log file compressed in one archive file".
  //########################################################################################//
  stage('Archiving Builds and build logs') {
            steps {
                sh '''
                   pwd
                   if [[ ! -e ArchiveBuilds/ ]]; then
                   mkdir ArchiveBuilds/
                   fi 
                   cd ArchiveBuilds/
                   curl -u jenkins:jenkins ${BUILD_URL}consoleText >> build${BUILD_NUMBER}.log
                   cd ..
                   cd HelloWorldSolution/
                   zip ubuntubuild.zip ubuntubuild/
                   mv ubuntubuild.zip  ubuntubuild${BUILD_NUMBER}.zip
                   mv ubuntubuild${BUILD_NUMBER}.zip ../ArchiveBuilds
                   cd ..
                   cd ArchiveBuilds/
                   zip ../HelloWorldSolution/ubuntubuild${BUILD_NUMBER}.zip build${BUILD_NUMBER}.log

                   '''
            }
        }
    }
}
