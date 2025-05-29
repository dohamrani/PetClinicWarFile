pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME' // Configure dans Jenkins > Global Tool Configuration
    }

    environment {
        ARTIFACTORY_URL = "http://192.168.50.40:8081/artifactory"
        ARTIFACTORY_REPO = "libs-release-local"
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/dohamrani/PetClinicWarFile.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Upload to Artifactory') {
            steps {
                script {
                    def server = Artifactory.server('artifactory-creds') // ID des credentials

                    def uploadSpec = """{
                      "files": [
                        {
                          "pattern": "target/*.war",
                          "target": "${ARTIFACTORY_REPO}/petclinic/"
                        }
                      ]
                    }"""

                    server.upload(uploadSpec)
                }
            }
        }

        stage('Archive WAR') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }
    }
}
