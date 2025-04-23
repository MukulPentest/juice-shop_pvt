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

//        stage('Clean .npmrc if exists') {
  //          steps {
    //            sh 'rm -f .npmrc'  // only if you suspect it's causing issues
      //      }
        //}

        stage('Install Dependencies') {
            steps {
                bash 'npm install --build-from-source'
            }
        }

        stage('Run App') {
            steps {
                bash 'nohup npm start &'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
