pipeline {
    agent none

    stages {
        stage('Build & Test') {
            agent {
                docker {
                    image 'gradle:8.14-jdk21'
                    // Double quotes allow Jenkins to inject the dynamic build number
                    args "--name spring-build-worker-${env.BUILD_NUMBER} -v ${WORKSPACE}/.gradle-cache:/home/gradle/.gradle"
                }
            }
            steps {
                echo "Running build inside container: spring-build-worker-${env.BUILD_NUMBER}"
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