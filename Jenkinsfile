pipeline {
    agent any
    tools {
        maven 'Maven3' // Reference the name you gave Maven in Global Tool Configuration
    }
    parameters {
        string(name: 'REPO_URL', defaultValue: 'https://github.com/achintyasingh109/jUNIT.git')
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build')
    }
    environment {
        EMAIL_RECIPIENTS = 'achintyasingh009@gmail.com'  // Replace with your recipient's email address
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
                   dir('jUNIT') {  // Change directory to the folder containing pom.xml
                    bat 'mvn clean install'
                }
                
            }
        }
        stage('Test') {
            steps {
                echo 'I am Testing...'
                dir('jUNIT') {  // Change directory to the folder containing pom.xml
                    bat 'mvn test'
                }
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
             dir('jUNIT') {  // Ensure we are in the correct directory to find the test reports
                junit '**/target/surefire-reports/*.xml'
            }
            echo 'Pipeline completed.'
            
        }
        success {
            echo 'Pipeline completed successfully.'
            // Email on success
            emailext (
                to: "${EMAIL_RECIPIENTS}",
                subject: "Jenkins Build Success: ${env.JOB_NAME} - ${env.BUILD_NUMBER}",
                body: "The build was successful.\n\n${BUILD_URL}"
            )
        }
        failure {
            echo 'Pipeline failed.'
            // Email on failure
            emailext (
                to: "${EMAIL_RECIPIENTS}",
                subject: "Jenkins Build Failure: ${env.JOB_NAME} - ${env.BUILD_NUMBER}",
                body: "The build failed.\n\n${BUILD_URL}\n\nCheck the Jenkins console output for details."
            )
        }
        unstable {
            echo 'Pipeline is unstable.'
            // Email on unstable build (e.g., failing tests)
            emailext (
                to: "${EMAIL_RECIPIENTS}",
                subject: "Jenkins Build Unstable: ${env.JOB_NAME} - ${env.BUILD_NUMBER}",
                body: "The build is unstable.\n\n${BUILD_URL}\n\nCheck the Jenkins console output for details."
            )
        }
        aborted {
            echo 'Pipeline was aborted.'
            // Email when the build is aborted
            emailext (
                to: "${EMAIL_RECIPIENTS}",
                subject: "Jenkins Build Aborted: ${env.JOB_NAME} - ${env.BUILD_NUMBER}",
                body: "The build was aborted.\n\n${BUILD_URL}"
            )
        }
}
}
