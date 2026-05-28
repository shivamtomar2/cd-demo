pipeline {

    agent none

    stages {

        stage('Checkout') {

            agent any

            steps {
                git 'https://github.com/shivamtomar2/cd-demo.git'
            }
        }

        stage('Build & Deploy to Staging') {

            agent {
                label 'staging'
            }

            steps {

                sh 'mvn clean package'

                sh '''
                ansible-playbook \
                -i inventory/staging \
                deploy.yml \
                --private-key ~/.ssh/Jenk.pem
                '''
            }
        }

        stage('Approval') {

            agent any

            steps {
                input 'Deploy to Production?'
            }
        }

        stage('Deploy to Production') {

            agent {
                label 'production'
            }

            steps {

                sh '''
                ansible-playbook \
                -i inventory/production \
                deploy.yml \
                --private-key ~/.ssh/Jenk.pem
                '''
            }
        }
    }
}
