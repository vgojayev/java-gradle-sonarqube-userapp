pipeline {
 
        agent {
                label 'master'
        }

        tools {
                gradle 'gradle'
        }

        environment {
                VERSION = "jellybean"
                SONARQUBE_HOSTNAME = 'localhost'
        }

        stages {
                stage('Checkout') {
                        steps {
                        checkout([$class: 'GitSCM',
                                        branches: [[name: 'master']],
                                        extensions: [[$class: 'WipeWorkspace']],
                                        userRemoteConfigs: [[url: 'https://github.com/vgojayev/java-gradle-sonarqube-userapp.git']]
                        
                        ])
                        }
                }


                stage('Details') {
                        steps {
                                echo "Runing ${env.BUILD_ID} on ${env.JENKINS_URL}"
                                echo "${env.VERSION}"
                        }

                        when {
                        environment name: 'VERSION', value: 'jellybean'
                         
                        }
                }

                stage('Build') {
                        steps {
                        sh "gradle build"
                }
                }
                stage('sonar-scanner') {
                 steps {
                  echo "Starting Scanner"
                  script {
                     def sonarqubeScannerHome = tool name: 'sonar', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                     withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
                     sh "${sonarqubeScannerHome}/bin/sonar-scanner -e -Dsonar.host.url=http://${SONARQUBE_HOSTNAME}:9000 -Dsonar.login=${sonarLogin} -Dsonar.projectName=UserMgmtApi -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=GS -Dsonar.sources=src/main/  -Dsonar.java.binaries=build/**/* -Dsonar.language=java"
                  }
                }
                }
                }
        }
}
