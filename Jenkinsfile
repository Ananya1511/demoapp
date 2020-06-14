pipeline {
           agent any
           stages {                
                stage("Build") {
                    steps {
                        bat 'gradlew clean build'
                    }
                }
                stage("Unit test") {
                    steps{
                        bat 'gradlew test'
                    }
                }
                stage("Static Code Analysis") {
                     steps{
                        bat 'gradlew lint'
                     }
                }
                stage('Static Security Analysis') {
                    steps{
                        //bat 'gradlew lint'
                        appscan application: '178d34b8-bdaa-45d9-b6eb-e013e8ebb040', 
                        credentials: 'hcl-app-scan', 
                        name: 'HCL-app-scan', 
                        scanner: mobile_analyzer(hasOptions: false, target: 'C:\\Users\\anapraka\\.jenkins\\workspace\\androidPipeline\\app\\build\\outputs\\apk\\debug\\app-debug.apk'),
                        type: 'Mobile Analyzer', 
                        wait: true
                    }
                }
                stage('Publish to App Center') {
                    environment {
                        APPCENTER_API_TOKEN = '48aa15089dd7040fb6dba262abdc9d66e370b821'
                    }
                    steps {
                        appCenter apiToken: APPCENTER_API_TOKEN, 
                        ownerName: 'ananyaprakash1511-gmail.com',
                        appName: 'demoapp',
                        pathToApp: 'app/build/outputs/apk/debug/app-debug.apk',
                        distributionGroups: 'Collaborators'                       
                    }
                }
           }
      }
