pipeline {
           agent any
           stages {
                      stage("Build") {
                                 steps {
                                            bat './gradlew clean build'
                                 }
                      }
                      stage("Unit Test") {
                                 steps{
                                            bat './gradlew test'
                                            junit '**/TEST-*.xml'
                                 }
                      }
                      stage("Static Code Analysis") {
                                 steps{
                                            bat './gradlew lint'
                                            androidLint pattern: '**/lint-results-*.xml'
                                 }
                      }
                      stage('SonarQube Analysis') {
                                 steps{
                                            withSonarQubeEnv('sonarqube') {
                                            bat './gradlew sonarqube'
                                            }
                                            timeout(time: 10, unit: 'MINUTES') {
                                                       waitForQualityGate abortPipeline: true
                                            }
                                            
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
