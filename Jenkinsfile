pipeline {
    agent any // Or specify a specific agent label

    tools {
        // Use the 'Name' you configured in Global Tool Configuration
        nodejs 'NodeJS-LTS'
        // You can add other tools here too, like:
        // jdk 'Java-17'
        // maven 'Maven-3.9'
    }

    stages {
        stage('Check Version') {
            steps {
                sh 'node --version'
                sh 'npm --version'
            }
        }
        stage('Build') {
            steps {
                echo 'Running npm install...'
                sh 'npm install' // npm commands are now available in PATH
                echo 'Running build...'
                sh 'npm run build' // Example build command
            }
        }
        // Add other stages like Test, Deploy, etc.
    }
}
