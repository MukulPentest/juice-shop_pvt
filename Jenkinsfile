pipeline {
    agent any

    environment {
        npm_config_build_from_source = 'true'
        npm_config_loglevel = 'verbose'
    }

    stages {
        stage('Clone from GitHub') {
            steps {
                git url: 'https://github.com/MukulPentest/juice-shop_pvt.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run App') {
            steps {
                sh 'npm start &'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
