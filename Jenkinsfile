pipeline {
    agent any

    tools {
        // Add relevant tools if needed, e.g., maven, node, etc.
    }

    stages {
        stage('Clone') {
            steps {
                git credentialsId: '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', url: 'https://github.com/MukulPentest/juice-shop_pvt.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Building..."'
                // Replace with actual build commands, e.g., npm install or mvn clean install
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Running Tests..."'
                // Replace with test commands
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo "Deploying..."'
                // Your deployment script or command
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }
    }
}
