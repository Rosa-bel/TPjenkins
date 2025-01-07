pipeline {
    agent any
    /*
    environment {
        GRADLE_HOME = '/usr/share/gradle'
        PATH = "${GRADLE_HOME}/bin:${env.PATH}"
    }
    */
    stages {
        stage('Test') {
            steps {
                // Run unit tests
                bat './gradlew test'

                // Archive test results
                junit 'build/test-results/test/*.xml'

                // Generate Cucumber reports
                cucumber jsonReportDirectory: 'build/reports/cucumber',
                         buildStatus: true
            }
        }

        /*
        stage('Code Analysis') {
            steps {
                // Run SonarQube analysis
                withSonarQubeEnv('sonar') {
                    bat './gradlew sonar'
                }
            }
        }

        stage('Quality Gates') {
            steps {
                // Wait for quality gate result
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build') {
            steps {
                // Build the project and generate JAR
                bat './gradlew build'

                // Generate documentation
                bat './gradlew javadoc'

                // Archive JAR and documentation
                archiveArtifacts artifacts: 'build/libs/*.jar, build/docs/javadoc/**', allowEmptyArchive: true
            }
        }

        stage('Deploy') {
            steps {
                // Publish to Maven repository
                bat './gradlew publish'
            }
        }
        */
    }
    /*
    post {
        success {
            // Notify on Slack about successful deployment
            slackSend(channel: '#deployments',
                      message: "Build and deployment of ${env.JOB_NAME} - ${env.BUILD_NUMBER} succeeded!",
                      webhookToken: 'your-slack-webhook-token')

            // Send email notification
            mail to: 'lr_belgacem@esi.dz',
                 subject: "Deployment Successful: ${env.JOB_NAME}",
                 body: "The deployment for build ${env.BUILD_NUMBER} has been completed successfully."
        }
        failure {
            // Notify on Slack about failure
            slackSend(channel: '#deployments',
                      message: "Build of ${env.JOB_NAME} - ${env.BUILD_NUMBER} failed!",
                      webhookToken: 'your-slack-webhook-token')

            // Send email notification
            mail to: 'lr_belgacem@esi.dz',
                 subject: "Deployment Failed: ${env.JOB_NAME}",
                 body: "The deployment for build ${env.BUILD_NUMBER} has failed. Please check the Jenkins logs for details."
        }
    }
    */
}
