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
    stage('Compile') {
      steps {
        // Compile the app and its dependencies
        bat './gradlew compileDebugSources'
      }
    }
    stage('Unit test') {
      steps {
        // Compile and run the unit tests for the app and its dependencies
        bat './gradlew test'

        // Analyse the test results and update the build result as appropriate
        junit '**/TEST-*.xml'
      }
    }
    stage('Build APK') {
      steps {
        // Finish building and packaging the APK
        bat './gradlew assembleDebug'

        // Archive the APKs so that they can be downloaded from Jenkins
        archiveArtifacts '**/*.apk'
      }
    }
    stage('Stage Archive') {
      steps {
        //tell Jenkins to archive the apks
        archiveArtifacts artifacts: 'app/build/outputs/apk/*.apk', fingerprint: true
      }
    }
    stage('Static analysis') {
      steps {
        // Run Lint and analyse the results
        bat './gradlew lintDebug'
        androidLint pattern: '**/lint-results-*.xml'
      }
    }
  } 
}
