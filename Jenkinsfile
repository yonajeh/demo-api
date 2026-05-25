pipeline {
    agent none

    stages {
        stage('Build & Test') {
            agent {
                docker {
                    image 'gradle:8.14-jdk21'
                    // FIXED: Explicitly used env.WORKSPACE so Groovy reads it from the environment
                    args "--name spring-build-worker-${env.BUILD_NUMBER} -v ${env.WORKSPACE}/.gradle-cache:/home/gradle/.gradle"
                }
            }
            steps {
                echo "Building Spring Boot (Java 21) Application with Gradle inside spring-build-worker-${env.BUILD_NUMBER}..."
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