pipeline {
    agent any
        stages {
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
    
