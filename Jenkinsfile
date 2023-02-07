pipeline {
 
        def SONARQUBE_HOSTNAME = 'localhost'

        def GRADLE_HOME = tool name: 'gradle', type: 'hudson.plugins.gradle.GradleInstallation'
        agent {
                label 'master'
        }

        tools {
                gradle 'gradle'
        }

        environment {
                VERSION = "jellybean"
        }

        stages {
                stage('Checkout') {
                        steps {
                        checkout([$class: 'GitSCM',
                                        branches: [[name: 'master']],
                                        extensions: [[$class: 'WipeWorkspace']],
                                        userRemoteConfigs: [[url: 'https://github.com/vgojayev/usermgmt-jenkins-lab.git']]
                        
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
                  def sonarqubeScannerHome = tool name: 'sonar', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                  withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
                    sh "${sonarqubeScannerHome}/bin/sonar-scanner -e -Dsonar.host.url=http://${SONARQUBE_HOSTNAME}:9000 -Dsonar.login=${sonarLogin} -Dsonar.projectName=WebApp -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=GS -Dsonar.sources=src/main/  -Dsonar.java.binaries=build/**/* -Dsonar.language=java"
                  }
                }
        }
}
