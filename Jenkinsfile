pipeline {
    agent any
    parameters {
        choice(name: 'DEPLOY', choices: ['V1', 'V2'], description: 'Which Version you want to deploy ?')
    }
 
    stages {
        stage("Checkout") {
            steps {
                script {
                if (params.DEPLOY == 'V1') {
                    git branch: 'amit/v1', url: 'https://github.com/amitmaurya07/nodejs-application.git'
                } else if (params.DEPLOY == 'V2') {
                    git branch: 'amit/v2', url: 'https://github.com/amitmaurya07/nodejs-application.git'
                } else {
                    sh 'echo "No Parameter has been selected"'
                }
                }
            }
        }
 
        stage("Ansible Ping") {
            steps {
                script {
                    sh 'ansible all -i ./ansible/inventory.txt -m ping'
                }
            }
        }
 
        stage("Ansible Playbook") {
            steps {
                script {
                    if (params.DEPLOY == 'V1') {
                        sh 'ansible-playbook -i ./ansible/inventory.txt ./ansible/version1.yaml'
                    } else if (params.DEPLOY == 'V2') {
                        sh 'ansible-playbook -i ./ansible/inventory.txt ./ansible/version2.yaml'
                    } else {
                        error "Invalid DEPLOY parameter value: ${params.DEPLOY}"
                    }
                }
            }
        }
    }
}