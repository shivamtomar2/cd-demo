pipeline {

    agent none

    stages {

        stage('Build & Deploy to Staging') {

            agent {
                label 'staging'
            }

            steps {

                sh 'mvn clean package'

                sh '''
                ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook \
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
                ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook \
                -i inventory/production \
                deploy.yml \
                --private-key ~/.ssh/Jenk.pem
                '''
            }
        }
    }
}
