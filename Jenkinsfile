pipeline {
           agent any
           stages {
                stage("Git checkout") {
                     steps {
                          git "https://github.com/Ananya1511/demoapp.git"
                     }
                }
                stage("Unit tests") {
                    steps{
                        bat 'gradlew test'
                    }
                }
                stage("Build") {
                    steps {
                        bat 'gradlew clean build -x test'
                    }
                }
              /*        stage('Static Code Analysis') {
                    steps{
                        //bat 'gradlew lint'
                        appscan application: 'c970c5a2-a6dc-4019-9231-a5156f584d45',
                                   credentials: 'HCL APP SCAN', name: 'HCL APP SCAN',
                                   scanner: mobile_analyzer(hasOptions: false,
                                                            target: 'C:/Program Files (x86)/Jenkins/workspace/My First Android Pipeline/app/build/outputs/apk/debug/app-debug.apk'),
                                   type: 'Mobile Analyzer'
                    }
                }*/
                stage('Publish to App Center') {
                    environment {
                        APPCENTER_API_TOKEN = 'a1c099b2f00a001f465bf9b36b4374fa7c29fa85'
                    }
                    steps {
                        appCenter apiToken: APPCENTER_API_TOKEN, 
                        appName: 'demoapp',
                        distributionGroups: 'Collaborators', 
                        ownerName: 'ananyaprakash1511-gmail.com', 
                        pathToApp: 'app/build/outputs/apk/debug/app-debug.apk'
                    }
                }
           }
      }
