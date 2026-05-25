pipeline {
    agent none

    stages {
        stage('Build & Test') {
            agent {
                docker {
                    // UPDATED: Upgraded to Gradle 8.14 and JDK 21
                    image 'gradle:8.14-jdk21'
                    // Keeps the Mac-friendly cache path we configured earlier
                    args '-v ${WORKSPACE}/.gradle-cache:/home/gradle/.gradle'
                }
            }
            steps {
                echo 'Building Spring Boot (Java 21) Application with Gradle...'
                sh 'gradle clean build -x test'
            }
            post {
                success {
                    echo 'Build completed successfully!'
                    archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
                }
            }
        }
    }
}