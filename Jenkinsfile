pipeline {
    agent any

    tools {
        // Make sure 'NodeJS-18' matches a NodeJS installation
        // configured in Jenkins -> Global Tool Configuration
        nodejs 'NodeJS-22'
    }

    stages {
        stage('Clone from GitHub') {
            steps {
                echo "Cloning repository..."
                // Replace with your actual URL, branch, and credential ID if repo is private
                git url: 'https://github.com/MukulPentest/juice-shop_pvt.git',
                    branch: 'master', // Or your desired branch
                    credentialsId: 'github-username-pat' // <<< REQUIRED for private repos
            }
        }

        stage('OWASP Dependency-Check Vulnerabilities') {
            steps {
                dependencyCheck additionalArguments: ''' 
                    -o './'
                    -s './'
                    -f 'ALL' 
                    --prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
        
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing Node.js dependencies..."
                // NodeJS tool ensures npm is in the PATH
                sh 'npm install'
            }
        }

        // stage('Run App') { // <<< RE-EVALUATE THIS STAGE
        //     steps {
        //         // Running in background like this is generally problematic in CI
        //         // Consider what the actual goal of this stage is.
        //         // If for testing: Start -> Run Tests -> Stop
        //         // If for deployment: Deploy artifact elsewhere after build/test.
        //         // sh 'npm start &'
        //         echo "Skipping 'npm start &' as background processes are not ideal here."
        //         echo "If you need to run tests against the app, structure accordingly."
        //         echo "If you need to deploy, do it in a later stage or separate job."
        //     }
        // }

        // Example: Stage for running tests (if 'npm test' exists)
        stage('Run Tests') {
            steps {
                 echo "Running tests..."
                 sh 'npm test' // Assumes your package.json has a 'test' script
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
            // cleanWs() // Optional: clean workspace
        }
    }
}
