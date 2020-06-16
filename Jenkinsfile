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
                stage('Sonarqube') {
                     environment {
                        def SCANNER_HOME = tool 'SonarQubeScanner';
                      }
                      steps {
                         withSonarQubeEnv('sonarqube') {
                         bat '${SCANNER_HOME}/bin/sonar-scanner'
                         }
                      timeout(time: 10, unit: 'MINUTES') {
                         waitForQualityGate abortPipeline: true
                         }
                      }
                 }
                  /*    stage('Sonar'){
                                 steps{
                                            bat 'gradlew sonarqube'
                                 }
                      }*/
                      
                      
           }
      }
