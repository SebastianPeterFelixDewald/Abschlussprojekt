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
        stage('deploy to Nexus') {
            steps {
            withCredentials([usernamePassword(credentialsId: 'nexus', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASSWORD')]) {
            configFileProvider([configFile(fileId: 'default', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                     sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS clean deploy'      
                    }
                }
            }
        }
        stage('Deploy War File to Local Tomcat') {
            steps {
                ansiblePlaybook inventory: 'inventory', colorized: true, installation: 'ansible2', playbook: 'deploy.yml', disableHostKeyChecking: true
                ansiblePlaybook inventory: 'inventory', colorized: true, installation: 'ansible2', playbook: 'remove.yml', disableHostKeyChecking: true
            }
        }
        }
    }