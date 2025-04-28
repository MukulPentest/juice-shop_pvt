pipeline {
    agent any

    tools {
        nodejs 'NodeJS-22'
    }

    stages {
        stage('Clone from GitHub') {
            steps {
                echo "Cloning repository..."
                git url: 'https://github.com/MukulPentest/juice-shop_pvt.git',
                    branch: 'master', // Or your desired branch
                    credentialsId: 'github-username-pat' // Ensure this exists
            }
        }

        stage('OWASP Dependency-Check Vulnerabilities') {
            steps {
                // Retry 3 times if the block fails
                retry(3) { 
                    timeout(time: 120, unit: 'MINUTES') { 
                withCredentials([string(credentialsId: 'nvdsecret', variable: 'nvdsecret')]) {
                    
                    echo "Running OWASP Dependency-Check with NVD API Key..."
                    
                    dependencyCheck additionalArguments: """ // Using triple double quotes for multi-line string
                        --nvdApiKey '${nvdsecret}' // Pass the key securely
                        --failOnCVSS 7 // Example: Optionally fail build on high severity CVEs
                        -o './dependency-check-reports' // Output to a dedicated directory
                        -s './' // Scan source/dependency files here
                        -f 'ALL' // Generate all report formats (consider XML/HTML if sufficient)
                        --prettyPrint
                        """.stripIndent(), // stripIndent helps with readability of multi-line strings
                        odcInstallation: 'OWASP-DC' // Ensure this matches your Jenkins Tool Configuration name

                    // Update the pattern to match the output directory '-o'
                    dependencyCheckPublisher pattern: 'dependency-check-reports/dependency-check-report.xml' 
                } // End of withCredentials block
            }
        }
            }

        stage('Install Dependencies') {
            steps {
                echo "Installing Node.js dependencies (using npm ci recommended)..."
                // Consider using 'npm ci' for better reproducibility in CI
                sh 'npm install' 
            }
        }

        stage('Build Application') {
             steps {
                 echo "Building application..."
                 // Assumes a 'build' script in package.json
                 sh 'npm run build' 
             }
        }

        stage('Run Tests') {
            steps {
                 echo "Running tests..."
                 // Assumes your package.json has a 'test' script
                 sh 'npm test' 
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
            // Optional: Archive reports for viewing in Jenkins build artifacts
            archiveArtifacts artifacts: 'dependency-check-reports/**', allowEmptyArchive: true
            // cleanWs() // Optional: clean workspace
            }
            success {
                echo 'Pipeline Succeeded'
            }
            failure {
                 echo 'Pipeline Failed'
            }
        }
    }
}
