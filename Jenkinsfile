pipeline {
    agent any
    options {
      timeout(time: 60, unit: 'SECONDS') 
    }
    stages {
                stage('Compile') {
                    steps {

                            sh './mvnw clean compile -e'
                        
                    }
                }
                stage('Unit') {
                    steps {

                            sh './mvnw clean test -e'

                    }
                }
                stage('Jar') {
                    steps {

                            sh './mvnw clean package -e'
                            sh 'pwd'

                    }
                }
                stage('SonarQube analysis') {
                    steps {

                            withSonarQubeEnv('sonar') {
                                sh './mvnw org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                            }

                    }
                }
                stage('Nexus Upload'){
                    steps{
                        nexusArtifactUploader artifacts: 
                        [
                            [
                                artifactId: 'DevOpsUsach2020', classifier: '', file: 'build/DevOpsUsach2020-1.0.1.jar', type: 'jar'
                            ]
                        ], 
                        credentialsId: 'nexus-local', 
                        groupId: 'com.devopsusach2020', 
                        nexusUrl: 'http://localhost:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'test-nexus', 
                        version: '1.0.1'
                    }
                }
                stage('Run') {
                    steps {

                            sh 'nohup bash ./mvnw spring-boot:run &'

                    }
                }
                stage('Test') {
                    steps {
                            sleep(time: 10, unit: "SECONDS")
                            sh 'curl -X GET "http://localhost:8081/rest/mscovid/test?msg=testing"'

                    }
                }
            
        }
}
