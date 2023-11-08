pipeline {
    agent any
    tools {
        ansible 'ansible'
    }

    stages {
        stage('git') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kirankumardudimetla/ansible-project.git']])
            }
        }
        stage('ansible') {
            steps {
                ansiblePlaybook credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory', playbook: 'playbook', vaultTmpPath: ''
            }
        }
    }
}
