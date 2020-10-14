pipeline {

    agent any
    environment {
        
        NEXUS_HOST = 'nexus:8081'
        SONAR_HOST = 'sonarqube:9000'
    }
        stages {
            stage ('start docker-compose') {
                steps {
                    sh 'docker-compose up -d --build'
                }
            }
            stage('test & compile') {
                steps {
                    sh 'mvn test' 
                }
            }
            stage('verify with Sonarqube') {
                steps {
                    configFileProvider([configFile(fileId: 'default', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                        sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS verify sonar:sonar'
                    }
                }
            }
            stage('create WAR-file') {
                steps {
                    sh 'mvn clean package -DskipTests'
                }
            }
            stage('deploy to nexus') {
                steps {
                    withCredentials([usernamePassword(credentialsId: 'nexus', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASSWORD')]) {
                    configFileProvider([configFile(fileId: 'default', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                        sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS deploy -DskipTests'
                        }
                    }
                }
            }

            stage("deploy War-file to tomcat") {
                steps {
                    ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory', playbook: 'deploy.yml'
            }
        }
            stage('stop docker-compose') {
                steps {
                    sleep (900)
                    sh 'docker-compose down'
                }
            }
    }
}