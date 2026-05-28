pipeline {

    agent any

    stages {

        stage('GitHub Source Checkout') {

            steps {

                echo 'GitHub → Source Code Management'

                checkout scm
            }
        }

        stage('Maven Build Automation') {

            steps {

                echo 'Maven → Building Java Application'

                sh 'mvn clean package'
            }
        }

        stage('Deploy to AWS Staging') {

            steps {

                echo 'Jenkins → Orchestrating Deployment'
                echo 'Ansible → Deploying to AWS Ubuntu Staging Server'

                sh '''
                ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook \
                -i inventory/staging \
                deploy.yml \
                --private-key ~/.ssh/Jenk.pem
                '''
            }
        }

        stage('Production Approval Gate') {

            steps {

                input 'Approve Production Deployment?'
            }
        }

        stage('Deploy to AWS Production') {

            steps {

                echo 'Ansible → Deploying to AWS Ubuntu Production Server'

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
