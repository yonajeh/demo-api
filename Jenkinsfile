pipeline {
    agent none // Do not allocate a global agent; define it per stage or block

    stages {
        stage('Build & Test') {
            agent {
                docker {
                    // This creates a separate worker container for this stage
                    image 'maven:3.9.6-eclipse-temurin-17'
                    // Reuses maven local repository cache on the host so builds stay fast
                    args '-v $HOME/.m2:/root/.m2'
                }
            }
            steps {
                echo 'Checking out code...'
                // Code is automatically checked out into the worker container workspace

                echo 'Building Spring Boot Application...'
                sh 'mvn clean package -DskipTests=false'
            }
            post {
                success {
                    echo 'Build completed successfully!'
                    // Optional: Archive the generated jar file
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
    }
}