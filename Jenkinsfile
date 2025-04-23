pipeline {
    agent any
 
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
}
