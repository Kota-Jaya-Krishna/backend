pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')     //pipeline will fail if it runs more than 10 sec//
        disableConcurrentBuilds()              // we should not run two build at same time, so we need to use disableconcurrentbuilds//
        retry(1)                               // if it fails, it will run 2nd time//
    }
    environment {
        DEBUG = 'true'
        appVersion = ''    // this will become global, we can use across the pipeline
    }

    stages {
        stage('Read the version') {
            steps {
                script { 
                    def packageJson = readJSON file: 'package.json'      // declaring the variable//
                    appVersion = packageJson.version
                    echo "App Verision: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Docker build') {
            steps {
                sh """
                docker build -t jayakrishna1993/backend:${appVersion} .
                docker images
                """
            }
        }
    }
        post { 
        always { 
            echo "This section run always"
            deleteDir()             // after pipeline is triggered, we can add deletedir function,it will delete the directory where jenkins file is there
        }
        success {
            echo "This section will run when pipeline success"
        }
        failure {
            echo "This section will run when pipeline failure"
        }
    }
}