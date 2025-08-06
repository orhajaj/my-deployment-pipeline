pipeline {
    agent any

    parameters {
        choice(name: 'ENV', choices: ['dev', 'stage', 'prod'], description: 'הסביבה שאליה נבצע דיפלוי')
        string(name: 'APP_VERSION', defaultValue: 'latest', description: 'גרסת האפליקציה ל-Docker')
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Pulling code from repository..."
                git branch: 'main',
                    credentialsId: 'github-token',
                    url: 'https://github.com/orhajaj/my-deployment-pipeline.git'
            }
        }

        stage('Deploy with Ansible') {
            steps {
                echo "Deploying to environment: ${params.ENV} with version: ${params.APP_VERSION}"
                sh """
                ansible-playbook -i ansible/inventory.ini ansible/deploy-playbook.yml \
                --extra-vars "env=${params.ENV} app_version=${params.APP_VERSION}"
                """
            }
        }
    }
}
