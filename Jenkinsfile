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
                    steps {
                        nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: 'localhost:8081',
                        groupId: 'com.devopsusach2020',
                        version: '0.0.1',
                        repository: 'test-nexus',
                        credentialsId: 'nexus-local',
                        artifacts: [
                            [artifactId: 'DevOpsUsach2020',
                            classifier: '',
                            file: 'build/DevOpsUsach2020-0.0.1.jar',
                            type: 'jar']
                        ]
                        )
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
