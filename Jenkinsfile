pipeline {
    // Specify a Node.js tool configured in Jenkins Global Tool Configuration
    // Replace 'NodeJS-18' with the actual name of your configured Node.js installation
    agent any

    tools {
        nodejs 'NodeJS-18' // Or 'nodejs-16', 'node18', etc. - MUST match Jenkins config
    }

    stages {
        stage('Clone from GitHub') {
            steps {
                // Clean workspace before cloning (optional, but good practice)
                cleanWs()
                git url: 'https://github.com/MukulPentest/juice-shop_pvt.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // 'sh' is generally preferred over 'bash' unless bash-specific features are needed
                // The 'nodejs' tool ensures 'npm' is in the PATH
                sh 'npm install --build-from-source'
            }
        }

        // Optional: Add a build or test stage if applicable
        // stage('Build') {
        //     steps {
        //         sh 'npm run build' // If your project has a build script
        //     }
        // }
        // stage('Test') {
        //     steps {
        //         sh 'npm test' // If your project has tests
        //     }
        // }

        // *** Important Note on Running the App ***
        // The 'Run App' stage using 'nohup' is removed because running applications
        // long-term directly on Jenkins agents is not recommended practice.
        // Instead, you should add stages here to:
        // 1. Build a deployable artifact (e.g., a Docker image).
        // 2. Deploy that artifact to a suitable environment (Server, Kubernetes, Cloud, etc.).

        // Example Placeholder: If you just wanted to *verify* it starts and exits
        // stage('Verify App Start') {
        //     steps {
        //         // Example: Run start command with a timeout or a specific check
        //         // This is highly dependent on how your app behaves
        //         echo "Skipping actual app run - add deployment steps instead."
        //     }
        // }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
            // Clean up workspace after build (optional)
            // cleanWs()
        }
    }
}
