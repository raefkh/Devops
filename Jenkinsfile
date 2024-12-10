

pipeline {
    agent any

    tools {
        maven 'M2_HOME'       // Ensure "M2_HOME" is configured in Jenkins
        jdk 'JAVA_HOME'       // Ensure "JAVA_HOME" is configured in Jenkins
    }

    environment {
        GIT_URL = 'https://github.com/raefkh/Devops.git'
        GIT_BRANCH = 'devops'
        CREDENTIALS_ID = 'GitHub_Credential'
        SONAR_TOKEN = credentials('SonarQube_Token')

    }


    stages {
        stage('Checkout Code') {
            steps {
                git branch: "${env.GIT_BRANCH}",
                    url: "${env.GIT_URL}",
                    credentialsId: "${env.CREDENTIALS_ID}"
            }
        }

        stage('Get Version') {
            steps {
                script {
                    env.APP_VERSION = sh(script: "mvn help:evaluate -Dexpression=project.version -q -DforceStdout", returnStdout: true).trim()
                    echo "Application version: ${env.APP_VERSION}"
                }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'  // This will compile and package the JAR
            }
        }
           stage('Mockito Tests') {
                            steps {
                                sh 'mvn test'
                            }
                        }
                stage('SonarQube Analysis') {
                              steps {
                                   sh 'mvn sonar:sonar'
                                  }
                              }
                               stage('Deploy to Nexus') {
                                                                  steps {
                                                                      script {
                                                                          withEnv(["PATH+MAVEN=${MAVEN_HOME}/bin"]) {
                                                                              sh 'mvn deploy -s /usr/share/maven/conf/settings.xml'
                                                                          }
                                                                      }
                                                                  }
                                                              }
    }
}
