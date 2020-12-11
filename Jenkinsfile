pipeline {
    agent any
    options {
      timeout(time: 60, unit: 'SECONDS') 
    }
    stages {
                stage('Download') {
                    steps {

                            sh 'curl -X GET -u admin:$admin_password http://localhost:8081/repository/test-nexus/com/devopsusach2020/DevOpsUsach2020/0.0.1/DevOpsUsach2020-1.0.0.jar -O'
                        
                    }
                }
                stage('Run') {
                    steps {

                            sh 'java -jar build/DevOpsUsach2020-0.0.1.jar &'

                    }
                }
                stage('Test') {
                    steps {
                            sleep(time: 10, unit: "SECONDS")
                            sh 'curl -X GET "http://localhost:8081/rest/mscovid/test?msg=testing"'

                    }
                }
                stage('Nexus Upload'){
                    steps {
                        nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: 'localhost:8081',
                        groupId: 'com.devopsusach2020',
                        version: '1.0.0',
                        repository: 'test-nexus',
                        credentialsId: 'nexus-local',
                        artifacts: [
                            [artifactId: 'DevOpsUsach2020',
                            classifier: '',
                            file: 'DevOpsUsach2020-0.0.1.jar',
                            type: 'jar']
                        ]
                        )
                        }
                }
                
        }
}
