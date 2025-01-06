pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                echo 'Running Unit Tests...'
                sh './gradlew test' // Run unit tests
                junit '**/build/test-results/test/*.xml' // Archive test results
                echo 'Generating Cucumber Reports...'
                // Example: Adjust the path based on your setup
                cucumber fileIncludePattern: '**/cucumber-reports/*.json'
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Analyzing Code with SonarQube...'
                withSonarQubeEnv('sonar') {
                    sh './gradlew sonarqube'
                }
            }
        }

        stage('Code Quality') {
            steps {
                echo 'Checking Quality Gates...'
                script {
                    def qg = waitForQualityGate() // Requires SonarQube Quality Gate plugin
                    if (qg.status != 'OK') {
                        error "Quality Gate failed: ${qg.status}"
                    }
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh './gradlew build' // Build the JAR file
                echo 'Generating Documentation...'
                sh './gradlew javadoc' // Generate JavaDocs
                echo 'Archiving Artifacts...'
                archiveArtifacts artifacts: '**/build/libs/*.jar, **/build/docs/**', fingerprint: true
            }
        }


}
