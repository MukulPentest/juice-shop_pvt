pipeline {
    agent any
 
    stages {
        stage('Clone from GitHub') {
            steps {
                git url: 'https://github.com/MukulPentest/juice-shop_pvt.git'
            }
        }
        
    environment {
        npm_config_build_from_source = 'true'
              npm_config_loglevel = 'verbose'
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
}
