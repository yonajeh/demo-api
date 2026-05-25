pipeline {
    agent none

    stages {
        stage('Build & Test') {
            agent {
                docker {
                    // Uses JDK 17 with Gradle
                    image 'gradle:8.6.0-jdk17'
                    // Caches Gradle dependencies on the host machine for faster builds
                    args '-v $HOME/.gradle:/home/gradle/.gradle'
                }
            }
            steps {
                echo 'Building Spring Boot Application with Gradle...'
                // Uses the gradle command inside the isolated container
                sh 'gradle clean build -x test'
            }
            post {
                success {
                    echo 'Build completed successfully!'
                    // Spring Boot Gradle plugin puts jars in build/libs/
                    archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
                }
            }
        }
    }
}