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
        bat 'gradlew test'

        // Analyse the test results and update the build result as appropriate
      //  junit '**/TEST-*.xml'
      }
    }
    stage('Build') {
      steps {
        // Compile the app and its dependencies
        bat 'gradlew clean build -x test'
      }
    }    
    
     stage('Publish to App Center') {
                    environment {
                        APPCENTER_API_TOKEN = '7943fdb1a13850de26e5f7b67599c3ff87a90cc5'//'bde0c2278c1177b8cbf9ffc34dc106ce7da66b24'
                    }
                    steps {
                        appCenter apiToken: APPCENTER_API_TOKEN, 
                        appName: 'My-Application',
                        distributionGroups: 'Collaborators, Testers', 
                        ownerName: 'pankajmishra', 
                        pathToApp: 'app/build/outputs/apk/debug/app-debug.apk'
                    }
                }
  } 
}
