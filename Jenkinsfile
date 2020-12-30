pipeline {
    agent any
    
    stages {
        stage('Restore packages'){
           steps{
               sh 'dotnet restore WebApplication.sln'
            }
         }        
        stage('Clean'){
           steps{
               sh 'dotnet clean WebApplication.sln --configuration Release  --no-restore'
            }
         }
        stage('Build'){
           steps{
               sh 'dotnet build WebApplication.sln --configuration Release --no-restore'
            }
         }
        stage('Test: Unit Test'){
           steps {
                sh 'dotnet test XUnitTestProject/XUnitTestProject.csproj --configuration Release --no-restore'
             }
          }
        stage('Publish'){
             steps{
               sh 'dotnet publish WebApplication/WebApplication.csproj --configuration Release --no-restore'
             }
        }
        stage('Deploy'){
             steps{
                 kill -9 $(lsof -t -i:9090)
                /usr/bin/nohup    dotnet WebApplication.dll --urls="http://104.128.91.189:9090" --ip="104.128.91.189" --port=9090 --no-restore  > /dev/null 2>&1 &
               sh 'cd WebApplication/bin/Release/netcoreapp3.1/publish/'
               sh 'dotnet WebApplication.dll --urls="http://104.128.91.189:9090" --ip="104.128.91.189" --port=9090 --no-restore'
             }
        }        
    }
}
