pipeline {
    agent any // Or your specific agent
    tools {
        nodejs 'NodeJS-LTS' // Assuming 'NodeJS-LTS' is configured in Jenkins Global Tool Config to point to v18, v20 or v22
    }
    stages {
        stage('Build') {
            steps {
                sh 'node -v'
                sh 'npm install'
                // ... other build steps
            }
        }
    }
}
