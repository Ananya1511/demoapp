pipeline {
  agent {
    // Run on a build agent where we have the Android SDK installed
    label "master"
  }
  options {
    // Stop the build early in case of compile or test failures
    skipStagesAfterUnstable()
  }
  
  stages {
    stage('Unit Test') {
      steps {
        // Compile and run the unit tests for the app and its dependencies
        bat './gradlew test'

        // Analyse the test results and update the build result as appropriate
        junit '**/TEST-*.xml'
      }
    }
    stage('Build') {
      steps {
        // Compile the app and its dependencies
        bat './gradlew clean build'
      }
    }    
    
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
