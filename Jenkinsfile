pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Shonty-devops/projCert.git'
            }
        }

        stage('Test') {
            steps {
                sh '''
                if [ -f website/index.php ]; then
                    echo "Test Passed: index.php exists"
                else
                    echo "Test Failed: index.php missing"
                    exit 1
                fi
                '''
            }
        }

        stage('Deploy') {
            steps {
                ansiblePlaybook(
                    playbook: 'website/playbook.yml',
                    inventory: 'inventory.ini'
                )
            }
        }
    }

    post {
        failure {
            echo "Cleaning up failed deploy..."
            sh 'docker rm -f php_webapp || true'
        }
    }
}

