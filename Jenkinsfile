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
    stage('Build') {
      steps {
        // Compile the app and its dependencies
        bat './gradlew clean compileDebugSources'
      }
    }
    stage('Unit Test') {
      steps {
        // Compile and run the unit tests for the app and its dependencies
        bat './gradlew test'

        // Analyse the test results and update the build result as appropriate
        junit '**/TEST-*.xml'
      }
    }
    stage('Static Security Analysis') {
      steps {
        // Run Lint and analyse the results
        bat './gradlew lintDebug'
        androidLint pattern: '**/lint-results-*.xml'
      }
    }
     stage('Build APK') {
      steps {
        // Finish building and packaging the APK
        bat './gradlew assembleDebug'

        // Archive the APKs so that they can be downloaded from Jenkins
        archiveArtifacts '**/demoapp.apk'
      }
    }
    stage('Stage Archive') {
      steps {
        //tell Jenkins to archive the apks
        archiveArtifacts artifacts: 'app/build/outputs/apk/demoapp.apk', fingerprint: true
      }
    }
    stage('Publish') {
      environment {
        APPCENTER_API_TOKEN = credentials('sample-api-token')
      }
      steps {
        appCenter apiToken: APPCENTER_API_TOKEN,
            ownerName: 'ananyaprakash1511-gmail.com',
            appName: 'demoapp',
            pathToApp: 'sample/demoapp.apk',
            distributionGroups: 'ap'
      }
    }
  } 
}
