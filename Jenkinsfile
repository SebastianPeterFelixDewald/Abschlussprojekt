pipeline {
    agent any
    environment {
        NEXUS_HOST = 'nexus:8081'}
        stages {
            stage('Testen und Kompilieren') {
                steps {
                    sh 'mvn clean test' 
                }
            }
           stage('Sonar Verify') {
                configFileProvider([configFile(fileId: 'default', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                    sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS clean verify sonar:sonar'
                }
            }
            stage('WAR-File erstellen') {
                steps {
                    sh 'mvn clean package -DskipTests'
                    sh 'ls -al target'
                }
            }
            stage('deploy to nexus') {
                steps {
                    withCredentials([usernamePassword(credentialsId: 'nexus', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASSWORD')]) {
                    configFileProvider([configFile(fileId: 'default', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                        sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS deploy'
                       } 
                    }
                }
            }
            stage("deploy War-file to tomcat") {
                steps {
                    ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory', playbook: 'deploy.yml'
            }
        }
    }
    }