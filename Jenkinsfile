pipeline {
    agent any 
    stages {
        stage('SCM checkout') {
            steps {
                echo 'git SCM checkout'
            }
        }
        stage('unit test') {
            steps {
                echo 'mvn test'
            }
        }

        stage('war-file package') {
            steps {
                echo 'war-file creation'
            }
        }
        stage('Deploy via Ansible') {
            steps {
                echo 'Run ansible.yml'
            }
        }
    }
}