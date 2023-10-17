// pipeline {
//     agent { node { label 'AGENT-1'} }
//     environment{
//         //here if you create any variable you will have global access, since it is environment no need of def
//         packageVersion = ''
//     }
//     // parameters {
//     //     string(name: 'version', defaultValue: '1.0.0', description: 'Who should I say hello to?')
//     // }
//     stages {
//         stage('Example') {
//             steps {
//                 echo "Hello ${params.version}"
//             }
//         }
//         stage('Get version'){
//             steps{
//                 script{
//                     def packageJson = readJSON(file: 'package.json')
//                     packageVersion = packageJson.version
//                     echo "version: ${packageVersion}"
//                 }
//             }
//         }
    
//         // stage('Install Dependencies') {
//         //     steps {
//         //         sh 'npm install'
//         //     }
//         // }

//         stage('Unit Test') {
//             steps {
//                 echo "unit test is done here"
//             }
//         } 
//         // sonar-scanner command expect sonar-project.properties should be available 
//         // stage('Sonar Scan') {
//         //     steps {
//         //         sh 'ls -ltr'
//         //         sh 'sonar-scanner'
//         //     }
//         // }
//         // stage('Build'){
//         //     steps {
//         //         sh 'ls -ltr'
//         //         sh 'zip -r catalogue.zip ./* --exclude=.git --exclude=.zip'
//         //     }
//         // }
//         // stage('publish artifact'){
//         //     steps {
//         //          nexusArtifactUploader(
//         //             nexusVersion: 'nexus3',
//         //             protocol: 'http',
//         //             nexusUrl: '52.87.248.172:8081/',
//         //             groupId: 'com.roboshop',
//         //             version: '1.0.9',
//         //             repository: 'catalogue',
//         //             credentialsId: 'nexus-auth',
//         //             artifacts: [
//         //                 [artifactId: 'catalogue',
//         //                  classifier: '',
//         //                  file: 'catalogue.zip',
//         //                  type: 'zip']
//         //             ]
//         //          )
//         //     }
//         // }

//         stage('SAST') {
//             steps {
//                 echo "SAST DONE"
//                 echo "package version: $packageVersion"
//             }
//         }
//         stage('D') {
//             steps {
//                 echo "Deployment"
//             }
//         }
   
//         // stage('deploy') {
//         //     steps {
//         //         echo "deployment"
//         //             buildjob: "../catelogue-deploy",wait: true
//         //     }
//         // }
//         stage('Deploy') {
//             steps {
//                 echo "Deployment"
//                 build job: "../catalogue-deploy", wait: true
//             }
//         }
    
//     }
//     post{
//         always{
//             echo 'cleaning up workspace'
//             deleteDir()
//         }
//     }
// }

pipeline {
    agent { node { label 'AGENT-1' } }
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
        stage('Install depdencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Unit test') {
            steps {
                echo "unit testing is done here"
            }
        }
        //sonar-scanner command expect sonar-project.properties should be available
        stage('Sonar Scan') {
            steps {
                echo "Sonar scan done"
            }
        }
        stage('Build') {
            steps {
                sh 'ls -ltr'
                sh 'zip -r catalogue.zip ./* --exclude=.git --exclude=.zip'
            }
        }
        stage('SAST') {
            steps {
                echo "SAST Done"
                echo "package version: $packageVersion"
            }
        }
        //install pipeline utility steps plugin, if not installed
        stage('Publish Artifact') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '172.31.86.20:8081/',
                    groupId: 'com.roboshop',
                    version: "$packageVersion",
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

        //here I need to configure downstram job. I have to pass package version for deployment
        // This job will wait until downstrem job is over
        stage('Deploy') {
            steps {
                script{
                    echo "Deployment"
                    def params = [
                        string(name: 'version', value: "$packageVersion")
                    ]
                    build job: "../catalogue-deploy", wait: true, parameters: params
                }
            }
        }
    }

    post{
        always{
            echo 'cleaning up workspace'
            //deleteDir()
        }
    }
}