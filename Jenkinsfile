pipeline {
    agent any
        stages {
            stage('Testen und Kompilieren') {
                steps {
                    sh 'mvn clean test' 
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

            stage('nexus war-file pull') {
        	    steps {
           		    echo 'curl -O -v http://nexus:8081/nexus/content/repositories/maven-snapshots/de.awacademy/spring-boot-webblog/0.0.1/spring-boot-webblog-0.1.1-SNAPSHOT.war'
                    echo 'ls -al'
        	    }
            }
            stage("deploy War-file to tomcat") {
                steps {
                    ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory', playbook: 'deploy.yml'
            }
        }
    }
}