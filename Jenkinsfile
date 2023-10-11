pipeline {
    agent { node { label 'AGENT-1'} }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Unit Test') {
            steps {
                echo "unit test is done here"
            }
        } 
        // sonar-scanner command expect sonar-project.properties should be available 
        // stage('Sonar Scan') {
        //     steps {
        //         sh 'ls -ltr'
        //         sh 'sonar-scanner'
        //     }
        // }
        stage('Build'){
            steps {
                sh 'ls -ltr'
                sh 'zip -r catalogue.zip ./* --exclude=.git --exclude=.zip'
            }
        }
        stage('publish artifact'){
            steps {
                 nexusArtifactUploader(
                    nexusVersion: 'nexus',
                    protocol: 'http',
                    nexusUrl: '52.87.248.172:8081/',
                    groupId: 'com.roboshop',
                    version: '1.0.0',
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: 'catalogue',
                         classifier: '',
                         file: 'catalogue.zip',
                         type: 'zip']
                    ]
                 )
            }
        }

        stage('Deploy') {
            steps {
                echo "Deployment"
            }
        }
    }

    post{
        always{
            echo 'cleaning up workspace'
            deleteDir()
        }
    }
}
        
        