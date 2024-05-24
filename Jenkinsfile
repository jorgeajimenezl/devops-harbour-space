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

        stage('Deploy') {
            steps {
                sh 'echo "Deploying..."'

                withCredentials([sshUserPrivateKey(credentialsId: 'mykey2',
                                                   keyFileVariable: 'mykey',
                                                   usernameVariable: 'myuser')]) {
                    sh 'ls -la'
                    // sh "echo -n '${mykey}' > ./mykey"

                    // sh 'chmod 600 ./mykey'

                    sh "scp -o StrictHostKeychecking=no -i ${mykey} main ${myuser}@192.168.105.3:"
                }
            }
        }
        stage('Run as a service') {
            steps {
                sh 'echo "Running as a service..."'
                withCredentials([sshUserPrivateKey(credentialsId: 'mykey2',
                                                   keyFileVariable: 'mykey',
                                                   usernameVariable: 'myuser')]) {
                    sh 'sudo systemctl stop myapp'

                    sh "scp -o StrictHostKeychecking=no -i ${mykey} myapp.service ${myuser}@192.168.105.3:"
                    // move unit file to systemd location
                    sh "sudo mv myapp.service /etc/systemd/system/"
                    sh "sudo systemctl daemon-reload"
                    sh "sudo systemctl start myapp"
                    sh "sudo systemctl status myapp"
                    sh "sudo systemctl enable myapp"
                }
            }
        }
    }
}
