pipeline {
    agent any
    environment {
        NEXUS_HOST = 'nexus:8081'
    }
    stages {
        stage('Testen & Kompilieren') {
            steps {
                        sh 'mvn test' 

                }

            }
        stage("WAR-File erstellen") {
            steps {
                        sh 'mvn clean package -DskipTests'
                }
            }
        stage("deploy War-file to tomcat") {
            steps {
                    ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory', playbook: 'deploy.yml'
                }
            }
        stage('Deploy to Nexus') {
            steps {
            withCredentials([usernamePassword(credentialsId: 'nexus', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASSWORD')]) {
            configFileProvider([configFile(fileId: 'f3ef7a41-a468-41f2-8819-7a02ecf6050b', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                        sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS clean deploy -DskipTests -DdeployOnly'        
                    }
                }
            }
        }
        }
    }