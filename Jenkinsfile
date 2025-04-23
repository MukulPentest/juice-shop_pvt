pipeline {
    agent any

    environment {
        npm_config_build_from_source = 'true'
        npm_config_loglevel = 'verbose'
    }

    stages {
        stage('Clone from GitHub') {
            steps {
                git url: 'https://github.com/username/reponame.git'
            }
        }

//        stage('Clean .npmrc if exists') {
  //          steps {
    //            sh 'rm -f .npmrc'  // only if you suspect it's causing issues
      //      }
        //}

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run App') {
            steps {
                sh 'nohup npm start &'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
