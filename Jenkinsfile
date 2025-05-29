pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME' // Assure-toi que ce nom est configuré dans Jenkins > Tools
    }

    environment {
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
                    def server = Artifactory.server('ce6c3ed8-6038-4735-9f24-9f2e890f559')

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
