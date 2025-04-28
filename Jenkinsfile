pipeline {
    agent any // Use any available agent. Consider specific agents (e.g., docker) for better control.

    tools {
        nodejs 'NodeJS-22' // Use the NodeJS installation configured in Jenkins Global Tools
        // The Dependency-Check plugin doesn't require a 'tools' entry here,
        // it uses the configuration from Global Tools via its step.
    }

    environment {
        // Optional: Define environment variables if needed
        // Example: CYPRESS_INSTALL_BINARY=0 // Might be needed to skip Cypress binary download if not running e2e tests
    }

   // options {
        // Optional: Add build timestamps, disable concurrent builds, etc.
     //   timestamps()
       // disableConcurrentBuilds()
        // Specify the Dependency-Check tool configured in Global Tool Config
        // This helps the plugin find the scanner installation
        //dependencyCheck installation: 'Default-DC'
    //}

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                // Uses the SCM definition from the Jenkins job configuration
                // For Pipeline jobs, this often requires explicit git step if not using Multibranch
                // For Multibranch, 'checkout scm' is implicit or can be used explicitly
                 checkout scm
                // OR, if using a standard Pipeline job and need explicit checkout:
                /*
                git branch: 'develop', // Or 'main', 'master', match your repo's default branch
                    credentialsId: 'github-pat', // The ID you gave your GitHub PAT credential in Jenkins
                    url: 'https://github.com/YOUR_USERNAME/juice-shop.git' // Replace YOUR_USERNAME
                */
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Node.js dependencies...'
                // Make sure npm is available via the NodeJS tool
                // Juice Shop uses npm ci for cleaner installs, requires package-lock.json
                // If package-lock.json is missing, might need 'npm install' first
                sh 'npm ci'
                // If 'npm ci' fails, try 'npm install' (might modify package-lock.json)
                // sh 'npm install'
            }
        }

        stage('Vulnerability Scan (Dependency-Check)') {
            steps {
                echo 'Running OWASP Dependency-Check...'
                dependencyCheckAnalyzer() // Runs the scan using configured settings

                // Publish the HTML report for easy viewing in Jenkins
                dependencyCheckPublisher pattern: '**/dependency-check-report.html'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
            // Clean up workspace if needed
            // cleanWs()
        }
        success {
            echo 'Pipeline succeeded!'
            // Add notifications (Slack, Email) if desired
        }
        failure {
            echo 'Pipeline failed!'
            // Add notifications for failures
        }
        unstable {
            // Dependency-Check often marks the build as UNSTABLE if vulnerabilities are found
            echo 'Pipeline unstable (vulnerabilities found?).'
        }
    }
}
