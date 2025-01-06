pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                echo 'Running Unit Tests...'
                sh './gradlew test' // Run unit tests

                echo 'Generating Cucumber Reports...'
                publishHTML(target: [
                    reportName: 'Cucumber Report',
                    reportDir: 'build/reports/cucumber',
                    reportFiles: 'report.json',
                    keepAll: true,
                    allowMissing: false,
                    alwaysLinkToLastBuild: true
                ])
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Analyzing Code with SonarQube...'
                withSonarQubeEnv('sonar') { // Replace 'sonar' with your configured SonarQube server name
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

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for details.'
        }
    }
}
