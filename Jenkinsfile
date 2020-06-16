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
                                            bat './gradlew lintDebug'
                                            androidLint pattern: '**/lint-results-*.xml'
                                 }
                      }
                      stage('SonarQube analysis') {
                                 steps{
                                            step{
                                            withSonarQubeEnv('sonarqube') {
                                            bat './gradlew sonarqube'
                                            }
                                            }
                                            step{
                                                       
                                            timeout(time: 10, unit: 'MINUTES') {
                                                       waitForQualityGate abortPipeline: true
                                            }}
                                            
                                 }
                      }
                      /*stage("Quality Gate"){
                                 steps{
                                            timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
                                                       def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                                                       if (qg.status != 'OK') {
                                                                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
                                                       }
                                            }
                                 }
                      }*/
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
