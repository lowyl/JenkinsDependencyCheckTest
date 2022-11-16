pipeline {
    agent any
    stages {
        stage ('Checkout') {
            steps {
            git 'https://github.com/lowyl/JenkinsDependencyCheckTest'
            }
        }
    
    stage('Code Quality Check via SonarQube') {
        steps {
            script {
            def scannerHome = tool 'SonarQube';
            withSonarQubeEnv('SonarQube') {
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=."
            }
            }
        }
    }
    }
    post {
        always {
            recordIssues enabledForFailure: true, tool: sonarQube()
        }
    }
}
