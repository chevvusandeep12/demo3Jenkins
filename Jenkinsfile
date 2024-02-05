pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Build the application using Maven
                    def mvnHome = tool 'Maven'
                    withMaven(maven: mvnHome, mavenSettingsConfig: 'maven-settings') {
                        sh 'mvn clean install'
                    }
                }
            }
        }

        stage('Static Code Analysis') {
            steps {
                script {
                    // Run SonarQube analysis
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }

        stage('Code Coverage') {
            steps {
                script {
                    // Run JaCoCo for code coverage
                    def mvnHome = tool 'Maven'
                    withMaven(maven: mvnHome, mavenSettingsConfig: 'maven-settings') {
                        sh 'mvn jacoco:prepare-agent test jacoco:report'
                    }
                    // Publish code coverage report to Jenkins
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'target/site/jacoco',
                        reportFiles: 'index.html',
                        reportName: 'JaCoCo Code Coverage'
                    ])
                }
            }
        }
    }

    post {
        success {
            // Notify on successful analysis and deployment
            emailext subject: 'Analysis and Deployment Successful',
                      body: 'The static code analysis and deployment have completed successfully.',
                      to: 'admin@example.com'
        }

        failure {
            // Notify on analysis or deployment failure
            emailext subject: 'Analysis or Deployment Failed',
                      body: 'The static code analysis or deployment has failed. Please investigate and fix.',
                      to: 'admin@example.com'
        }
    }
}
