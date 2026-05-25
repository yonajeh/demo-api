pipeline {
    agent none

    stages {
        stage('Build & Test') {
            agent {
                docker {
                    image 'gradle:8.14-jdk21'
                    // Removed the -v path flag to prevent the 'null' workspace evaluation error
                    args "--name spring-build-worker-${env.BUILD_NUMBER}"
                }
            }
            steps {
                echo "Building Spring Boot Application inside spring-build-worker-${env.BUILD_NUMBER}..."

                // --project-cache-dir forces Gradle to store its cache right inside the workspace directory,
                // which is automatically shared and safe on Docker Desktop for Mac.
                sh 'gradle clean build -x test --project-cache-dir .gradle-cache'
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