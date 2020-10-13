String maven = "maven:3.6.3-adoptopenjdk-14"
pipeline {
    agent any
    environment {
        NEXUS_HOST = 'nexus:8081'
        SONAR_HOST = 'sonarqube:8084'
    }
  stages {
        stage('Build') {
            steps {
                script {

                           }
        }
        stage('Sonar Verify Test') {
                configFileProvider([configFile(fileId: 'f3ef7a41-a468-41f2-8819-7a02ecf6050b', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                    sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS clean verify sonar:sonar'
                }
            }
        stage('Deploy to Nexus Repo') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASSWORD')]) {
                    configFileProvider([configFile(fileId: 'f3ef7a41-a468-41f2-8819-7a02ecf6050b', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                        sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS clean deploy -DskipTests -DdeployOnly'
                    }
                }

                }
            }
        }
            stage("WAR-File erstellen") {
                agent {
                    docker {
                        image 'maven'
                        args '--network miniprojekt'
                        }
                    }
                steps {
                        sleep (4)
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
    
