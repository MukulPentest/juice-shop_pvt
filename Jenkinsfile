pipeline {
    agent any // Use any available agent on your localhost Jenkins

    tools {
        nodejs 'NodeJS-22' // Match the name in Jenkins Global Tool Config
        // Dependency-Check tool is specified via the 'installation' option later
    }

    environment {
        // CYPRESS_INSTALL_BINARY=0 // uncomment if needed to potentially speed up npm install by skipping Cypress binary
    }

    options {
        timestamps()
        // Specify the Dependency-Check tool installation configured in Global Tool Config
        dependencyCheck installation: 'OWASP-DC' // Match the name in Jenkins Global Tool Config
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                // Explicit git step is clearer for standard Pipeline jobs
                git branch: 'master', // <<< CHANGE to your repo's main branch (e.g., 'main', 'master', 'develop')
                    credentialsId: '5a8139db-21e5-446d-a31d-77f4daf60940', // <<< CHANGE to your Jenkins credential ID for the GitHub PAT
                    url: 'https://github.com/MukulPentest/juice-shop_pvt.git' // <<< CHANGE YOUR_USERNAME
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Node.js dependencies...'
                // Ensure npm from the NodeJS tool is in PATH
                sh 'npm ci' // Use 'npm ci' for clean install using package-lock.json
                // Or use 'npm install' if 'npm ci' fails or package-lock.json is outdated/missing
                // sh 'npm install'
            }
        }

        stage('Vulnerability Scan (Dependency-Check)') {
            steps {
                echo 'Running OWASP Dependency-Check...'
                // Runs the scan using the globally configured tool ('Default-DC')
                // Adjust parameters here if needed, e.g., data directory, suppression file
                dependencyCheckAnalyzer()

                // Publish the HTML report
                dependencyCheckPublisher pattern: '**/dependency-check-report.html'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
            // cleanWs() // Optional: clean workspace after build
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        unstable {
            // Dependency-Check often marks the build as UNSTABLE if vulnerabilities are found
            echo 'Pipeline unstable (likely due to found vulnerabilities).'
        }
    }
}
