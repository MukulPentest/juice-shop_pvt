pipeline {
    agent any // Use any available agent on your localhost Jenkins

    tools {
        nodejs 'NodeJS-22' // Match the name in Jenkins Global Tool Config
        // Dependency-Check tool is specified via the 'installation' option later
    }

   //options {
     //   timestamps()
        // Specify the Dependency-Check tool installation configured in Global Tool Config
       // dependencyCheck installation: 'OWASP-DC' // Match the name in Jenkins Global Tool Config
   // }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                // Explicit git step is clearer for standard Pipeline jobs
                git branch: 'master', // <<< CHANGE to your repo's main branch (e.g., 'main', 'master', 'develop')
                    credentialsId: 'github-pat', // <<< CHANGE to your Jenkins credential ID for the GitHub PAT
                    url: 'https://github.com/MukulPentest/juice-shop_pvt.git' // <<< CHANGE YOUR_USERNAME
            }
        }

            stage('Install Dependencies') {
                steps {
                    echo 'Attempting to remove potentially problematic node_modules directory...'
                    // Add this line to clean before installing
                    sh 'rm -rf node_modules'
                    // It's also safe to remove the lock file if npm install is going to regenerate it anyway
                    sh 'rm -f package-lock.json'
    
                    echo 'Installing Node.js dependencies...'
                    // Now run npm install
                    sh 'npm install'
                }
        }
        
        stage('npm Audit') {
            steps {
                echo 'Running npm Audit...'
                // Ensure npm from the NodeJS tool is in PATH
                sh 'npm audit'
                // Or use 'npm install' if 'npm ci' fails or package-lock.json is outdated/missing
                // sh 'npm install'
            }
        }
       // stage('Vulnerability Scan (Dependency-Check)') {
         //   steps {
           //     echo 'Running OWASP Dependency-Check...'
                // Runs the scan using the globally configured tool ('Default-DC')
                // Adjust parameters here if needed, e.g., data directory, suppression file
              //  dependencyCheckAnalyzer()

                // Publish the HTML report
                //dependencyCheckPublisher pattern: '**/dependency-check-report.html'
     //       }
       // }
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
