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
        stage('Deploy War File to Local Tomcat') {
            steps {
                ansiblePlaybook inventory: 'inventory', colorized: true, installation: 'ansible2', playbook: 'deploy.yml', disableHostKeyChecking: true
                ansiblePlaybook inventory: 'inventory', colorized: true, installation: 'ansible2', playbook: 'remove.yml', disableHostKeyChecking: true
            }
        }
        stage('Deploy to Azure') {
                withCredentials([azureServicePrincipal(credentialsId: env.AZURE_CRED_ID,
                            subscriptionIdVariable: 'AZURE_SUBSCRIPTION_ID',
                            clientIdVariable: 'AZURE_CLIENT_ID',
                            clientSecretVariable: 'AZURE_SECRET',
                            tenantIdVariable: 'AZURE_TENANT')]) {
                ansiblePlaybook installation: 'ansible2', playbook: 'webapp.yml'
                            }
                azureWebAppPublish azureCredentialsId: env.AZURE_CRED_ID,
                            resourceGroup: env.RES_GROUP,
                            appName: env.WEB_APP_NAME,
                            filePath: "ROOT.war"
         }
    }
}

    
