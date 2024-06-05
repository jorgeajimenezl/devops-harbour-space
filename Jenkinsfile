pipeline {
    agent any

    tools {
        go 'go-1.22'
    }

    stages {
        stage('Build') {
            steps {
                sh 'go build main.go'
                sh 'ls -la'
                sh 'echo "Build done!!!"'
            }
        }
        stage('Deploy to Stage') {
            environment {
                ANSIBLE_HOST_KEY_CHECKING = 'false'
            }
            steps {
                sh 'echo "Deploying to stage..."'
                // ansiblePlaybook credentialsId: 'mykey2',
                //                 inventory: 'staging.ini',
                //                 playbook: 'playbook.yml'
            }
        }
        stage('Deploy to Production') {
            environment {
                ANSIBLE_HOST_KEY_CHECKING = 'false'
            }
            steps {
                sh 'echo "Deploying to production..."'
                // ansiblePlaybook credentialsId: 'aws-key',
                //                 inventory: 'production.ini',
                //                 playbook: 'playbook.yml'
            }
        }
    }
}
