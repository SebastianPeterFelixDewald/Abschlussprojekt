pipeline {
    agent any
    /*environment {
        NEXUS_HOST = 'nexus:8081'
        SONAR_HOST = 'sonarqube:9000'
        TOMCAT_HOST = 'tomcat:8080'
        }*/
        stages {
            /*stage ('start docker-compose') {
                steps {
                    sh 'docker-compose up -d --build'
                }
            }*/
            stage('compile') {
                steps {
                    echo "${WORKSPACE}"
                        script {
                            mvn.compile() 
                        }
                    }
            }
            stage('test') {
                steps {
                    script {
                        mvn.test()
                    }
                }
            }
            stage('verify with Sonarqube') {
                steps {
                    script {
                        mvn.verify()
                    }
                }
            }
            stage('create WAR-file') {
                steps {
                    script {
                      mvn.artifactpackage()
                    }
                }
            }
            stage('deploy to nexus') {
                steps {
                    withCredentials([usernamePassword(credentialsId: 'nexus', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASSWORD')]) {
                        configFileProvider([configFile(fileId: 'default', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                        script {
                            mvn.deploy()
                               }
                        }
                    }
                }
            }
            /*stage('deploy War-file to tomcat') {
                steps {
                    withCredentials([usernamePassword(credentialsId: 'tomcat', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASSWORD')]) {
                    configFileProvider([configFile(fileId: 'default', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                      sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS tomcat7:redeploy -DskipTests'
                        }
                    }
                }
            }
            stage('stop docker-compose') {
                steps {
                    sleep(300)
                    sh 'docker-compose down'
                }
            }*/
    }
}
    