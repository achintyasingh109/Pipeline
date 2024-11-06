pipeline {
    agent any
    tools {
        maven 'Maven3' // Reference the name you gave Maven in Global Tool Configuration
    }
    parameters {
        string(name: 'REPO_URL', defaultValue: 'https://github.com/achintyasingh109/jUNIT.git')
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build')
    }
    

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning from repository ${params.REPO_URL} on branch ${params.BRANCH}"
                // Pull the repository
                git branch: "${params.BRANCH}", url: "${params.REPO_URL}"
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
                    bat 'mvn clean install'
                
            }
        }
        stage('Test') {
            steps {
                echo 'I am Testing...'
                bat 'mvn test'
                // Here, include commands to run your test files, e.g., `npm test` or `pytest`
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
    post {
        always {
            junit '**/target/surefire-reports/*.xml'
            echo 'Pipeline completed.'
            
        }
        success{
            echo 'Pipeline successfully build.'
        }
        failure{
            echo 'Pipeline failed.'
        }
    }
}
