pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', credentialsId: 'git-tinht', url: 'https://github.com/tinht/jenkins-demo.git'

                // Run Maven on a Unix agent.
                //sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        
        stage('Test') {
            steps {
                bat 'mvn test -Pskip-test'
            }
        }
    }
    
    post {
        always {
            junit 'build/reports/**/*.xml'
        }
    }
}
