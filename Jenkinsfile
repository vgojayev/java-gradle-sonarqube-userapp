pipeline {
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
        }
}
