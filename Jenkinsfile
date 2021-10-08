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
            def reportPath = "${WORKSPACE}/target/surefire-reports"
            echo "${currentBuild.fullDisplayName} - ${env.BUILD_URL}"
            echo "Report saved at ${reportPath}"
            junit 'build/reports/**/*.xml'
        }
        
        success {
            echo 'I succeeded!'
             mail to: 'ttsoft.vn@gmail.com',
                 subject: "Test's finished: ${currentBuild.fullDisplayName}",
                 body: "Check more detail at ${env.BUILD_URL}"
        }
        
        unstable {
            echo 'I am unstable :/'
        }
        
        failure {
            echo 'I failed :('
        }
        
        success  {
            mail to: 'ttsoft.vn@gmail.com',
                 subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                 body: "Something is wrong with ${env.BUILD_URL}"
        }
    }
}
