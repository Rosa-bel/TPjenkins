pipeline {
    agent any

   /** environment {
        GRADLE_HOME = '/usr/share/gradle'
        PATH = "${GRADLE_HOME}/bin:${env.PATH}"
    }**/

    stages {

     stage('Test') {
                steps {
                    script {

                        bat './gradlew test'

                    }
                }
                post {
                    always {

                        junit 'build/test-results/test/*.xml'

                        cucumber 'reports/example-report.json'
                    }
                }
            }


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

                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build') {
            steps {

                bat './gradlew build'


                bat './gradlew javadoc'
                archiveArtifacts artifacts: 'build/libs/*.jar, build/docs/javadoc/**', allowEmptyArchive: true
            }
        }

        stage('Deploy') {
            steps {

                bat './gradlew publish'
            }
        }


}

 post {
        success {

            slackSend(channel: '#test',
                      message: "Build and deployment of ${env.JOB_NAME} - ${env.BUILD_NUMBER} succeeded!",
                      webhookToken: 'TNThHQevSAO0B1EPjYi6M7JE')


            mail to: 'lr_belgacem@esi.dz',
                 subject: "Deployment Successful: ${env.JOB_NAME}",
                 body: "The deployment for build ${env.BUILD_NUMBER} has been completed successfully."
        }
        failure {

            slackSend(channel: '#test',
                      message: "Build of ${env.JOB_NAME} - ${env.BUILD_NUMBER} failed!",
                      webhookToken: 'TNThHQevSAO0B1EPjYi6M7JE')


            mail to: 'lr_belgacem@esi.dz',
                 subject: "Deployment Failed: ${env.JOB_NAME}",
                 body: "The deployment for build ${env.BUILD_NUMBER} has failed. Please check the Jenkins logs for details."
        }

    }

}
