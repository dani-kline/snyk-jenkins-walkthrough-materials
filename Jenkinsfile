pipeline {
    agent any

    // Install the Jenkins tools you need for your project / environment
    tools {
        nodejs 'nodeInstallationName' // Refers to a global tool configuration for nodejs called 'nodeInstallationName' 
    }

    stages {

        stage('Initialize & Cleanup Workspace') {
            steps {
               echo 'Initialize & Cleanup Workspace'
               sh 'ls -la'
               sh 'rm -rf *'
               sh 'rm -rf .git'
               sh 'rm -rf .gitignore'
               sh 'ls -la'
            }
        }

        stage('Git Clone') {
            steps {
                git url: 'https://github.com/dani-kline/goof.git'
                sh 'ls -la'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install browserify'
                sh 'npm run build'
            }
        }

        // Run snyk test to check for vulnerabilities and fail the build if any are found
        stage('Snyk Test using Snyk CLI') {
            steps {
                snykSecurity(
                    snykInstallation: 'snykInstallationName',
                    snykTokenId: 'snykTokenId',
                    organisation: 'orgId',
                    // projectName is commented out as it was causing issues when running filter
                    // projectName: 'jenkins-pipeline-walkthrough',
                    monitorProjectOnBuild: false,
                    // additionalArguments: '--json | snyk-filter -f snyk-filter/exploitable_cvss_9.yml',
                    // place other parameters here
                )
            }
        }
    }
}
