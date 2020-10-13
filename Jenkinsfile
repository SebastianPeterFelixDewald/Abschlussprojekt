String maven = "maven:3.6.3-adoptopenjdk-14"
pipeline {
    agent any
        stages {
            stage("Run Docker-compose") {
                steps {
                    sh "docker-compose up -d"
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
    
