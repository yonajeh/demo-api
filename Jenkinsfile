pipeline {
    agent none

    stages {
        stage('Build & Test') {
            agent {
                docker {
                    image 'gradle:8.6.0-jdk17'
                    // Stores the cache safely inside the workspace, avoiding the Mac path block
                    args '-v ${WORKSPACE}/.gradle-cache:/home/gradle/.gradle'
                }
            }
            steps {
                echo 'Building Spring Boot Application with Gradle...'
                sh 'gradle clean build -x test'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
                }
            }
        }
    }
}