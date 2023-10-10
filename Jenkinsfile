pipeline {
    agent { node { label 'AGENT-1'} }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        //sonar-scanner command expect sonar-project.properties should be available 
        stage('Sonar Scan') {
            steps {
                sh 'ls -ltr'
                sh 'sonar-scanner'
            }
        }
        stage('deploy') {
            steps {
                echo "deployment"
            }
        }
    }
}