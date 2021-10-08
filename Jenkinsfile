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

                bat "mvn surefire-report:report-only"
            }
        }
        
//         stage('Test') {
//             steps {
//                 bat 'mvn test -Pskip-test'
//             }
//         }
    }
    
    post {
        always {
            echo "${currentBuild.fullDisplayName} - ${env.BUILD_URL}"
            echo "Report saved at ${WORKSPACE}/target/surefire-reports"
            junit allowEmptyResults: true, testResults: '**/surefire-reports/*.xml'

            publishHTML([allowMissing: false,
                 alwaysLinkToLastBuild: true,
                 keepAll: true,
                 reportDir: '**/site',
                 reportFiles: 'surefire-report.html',
                 reportName: 'Docs Loadtest Dashboard'
                 ])
        }
        
        success {
            echo 'I succeeded!'
        }
        
        unstable {
            echo 'I am unstable :/'
        }
        
        failure {
            echo 'I failed :('
        }
    }
}
