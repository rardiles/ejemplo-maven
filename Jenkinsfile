pipeline {
    agent any
    options {
      timeout(time: 120, unit: 'SECONDS') 
    }
    stages {
                stage('Compile') {
                    steps {
                        dir('/Users/ricardoardiles/Documents/Diplomado/modulo3git/ejemplo-maven'){
                            sh './mvnw clean compile -e'
                        }
                    }
                }
                stage('Unit') {
                    steps {
                        dir('/Users/ricardoardiles/Documents/Diplomado/modulo3git/ejemplo-maven'){
                            sh './mvnw clean test -e'
                        }
                    }
                }
                stage('Jar') {
                    steps {
                        dir('/Users/ricardoardiles/Documents/Diplomado/modulo3git/ejemplo-maven'){
                            sh './mvnw clean package -e'
                        }
                    }
                }
                stage('Run') {
                    steps {
                        dir('/Users/ricardoardiles/Documents/Diplomado/modulo3git/ejemplo-maven'){
                            sh 'nohup bash ./mvnw spring-boot:run &'
                        }
                    }
                }
                stage('Test') {
                    steps {
                        dir('/Users/ricardoardiles/Documents/Diplomado/modulo3git/ejemplo-maven'){
                            sh 'curl -X GET "http://localhost:8081/rest/mscovid/test?msg=testing"'
                        }
                    }
                }
            
        }
}
