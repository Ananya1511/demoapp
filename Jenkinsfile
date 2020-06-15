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
                
           }
      }
