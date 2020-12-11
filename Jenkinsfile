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
                                        nexusPublisher nexusInstanceId: 'nexus-local', nexusRepositoryId: 'test-nexus', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: 'build/DevOpsUsach2020-1.0.2.jar', extension: 'jar', filePath: '']], mavenCoordinate: [artifactId: 'DevOpsUsach2020', groupId: 'com.devopsusach2020', packaging: 'jar', version: '1.0.0']]]

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
