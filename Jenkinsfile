pipeline {
    agent any
    options {
      timeout(time: 60, unit: 'SECONDS') 
    }
    stages {
                stage('Compile') {
                    steps {
                        dir('/Users/ricardoardiles/Desktop/Personal/Diplomado/taller4/ejemplo-maven'){
                            sh './mvnw clean compile -e'
                        }
                    }
                }
                stage('Unit') {
                    steps {
                        dir('/Users/ricardoardiles/Desktop/Personal/Diplomado/taller4/ejemplo-maven'){
                            sh './mvnw clean test -e'
                        }
                    }
                }
                stage('Jar') {
                    steps {
                        dir('/Users/ricardoardiles/Desktop/Personal/Diplomado/taller4/ejemplo-maven'){
                            sh './mvnw clean package -e'
                        }
                    }
                }
                stage('SonarQube analysis') {
                    withSonarQubeEnv('sonar') {
                    sh './mvnw org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                }
                stage('Run') {
                    steps {
                        dir('/Users/ricardoardiles/Desktop/Personal/Diplomado/taller4/ejemplo-maven'){
                            sh 'nohup bash ./mvnw spring-boot:run &'
                        }
                    }
                }
                stage('Test') {
                    steps {
                        dir('/Users/ricardoardiles/Desktop/Personal/Diplomado/taller4/ejemplo-maven'){
                            sh 'curl -X GET "http://localhost:8081/rest/mscovid/test?msg=testing"'
                        }
                    }
                }
            
        }
}
