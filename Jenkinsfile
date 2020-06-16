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
                stage('SonarQube') {
                     environment {
                        scannerHome = tool 'SonarQubeScanner'
                     }
                     steps {
                        withSonarQubeEnv('sonarqube') { 
                        //bat "${scannerHome}/bin/sonar-scanner.bat"
                        bat 'gradle --info sonarqube  -Dsonar.projectKey=catalog-service -Dsonar.junit.reportPaths=./build/test-results/test -Dsonar.binaries=./build/classes -Dsonar.coverage.jacoco.xmlReportPaths=./build/reports/jacoco/test/html/index.html'
           
                        }
                     }
                }
               /* stage('Sonarqube') {
                     def scannerHome = tool 'SonarScanner 4.0';
                      //steps {
                         withSonarQubeEnv('sonarqube') {
                         bat '${SCANNER_HOME}/bin/sonar-scanner'
                         }
                      //timeout(time: 10, unit: 'MINUTES') {
                        // waitForQualityGate abortPipeline: true
                         //}
                      }
                 }*/
                      /*stage('Sonar'){
                                 steps{
                                            bat 'gradle sonarqube'
                                 }
                      }*/
                     
                      
                      
           }
      }
