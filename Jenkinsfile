pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME' // Assure-toi que ce nom est configuré dans Jenkins > Global Tools
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

        stage('Archive WAR') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }
    }
}
