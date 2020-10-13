pipeline {
    agent any
    environment {
        NEXUS_HOST = 'nexus:8081'
    }
    stages {
        stage('Deploy to Nexus') {
            steps {
            withCredentials([usernamePassword(credentialsId: 'nexus', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASSWORD')]) {
            configFileProvider([configFile(fileId: 'default', variable: 'MAVEN_GLOBAL_SETTINGS')]) {        
                        sh 'mvn $MAVEN_GLOBAL_SETTINGS clean deploy -DskipTests -DdeployOnly'
                    }
                }
            }
        }
        stage("WAR-File erstellen") {
            steps {
                        sh 'mvn clean package'
                }
            }
        stage("deploy War-file to tomcat") {
            steps {
                    ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory', playbook: 'deploy.yml'
                }
            }
        }
    }