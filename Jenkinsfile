pipeline {
    agent { node { label 'AGENT-1'} }
    environment{
        //here if you create any variable you will have global access, since it is environment no need of def
        packageVersion = ''
    }
    stages {
        stage('Get version'){
            steps{
                script{
                    def packageJson = readJSON(file: 'package.json')
                    packageVersion = packageJson.version
                    echo "version: ${packageVersion}"
                }
            }
        }
        // stage('Install Dependencies') {
        //     steps {
        //         sh 'npm install'
        //     }
        // }

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
        // stage('Build'){
        //     steps {
        //         sh 'ls -ltr'
        //         sh 'zip -r catalogue.zip ./* --exclude=.git --exclude=.zip'
        //     }
        // }
        // stage('publish artifact'){
        //     steps {
        //          nexusArtifactUploader(
        //             nexusVersion: 'nexus3',
        //             protocol: 'http',
        //             nexusUrl: '52.87.248.172:8081/',
        //             groupId: 'com.roboshop',
        //             version: '1.0.9',
        //             repository: 'catalogue',
        //             credentialsId: 'nexus-auth',
        //             artifacts: [
        //                 [artifactId: 'catalogue',
        //                  classifier: '',
        //                  file: 'catalogue.zip',
        //                  type: 'zip']
        //             ]
        //          )
        //     }
        // }

        stage('SAST') {
            steps {
                echo "SAST DONE"
                echo "package version: $packageVersion"
            }
        }
        stage('D') {
            steps {
                echo "Deployment"
            }
        }
   
        // stage('deploy') {
        //     steps {
        //         echo "deployment"
        //             buildjob: "../catelogue-deploy",wait: true
        //     }
        // }
        stage('Deploy') {
            steps {
                echo "Deployment"
                build job: "../catalogue-deploy", wait: true
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

        