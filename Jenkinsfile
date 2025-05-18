
pipeline {
    agent any

    tools {
        sonarScanner 'SonarScanner'  // اسم SonarScanner في Jenkins Global Tools
    }

    environment {
        SONAR_PROJECT_KEY = 'my-sample-project'   // غيره باسم مشروعك في SonarQube
        SONAR_HOST_URL = 'http://localhost:5100'  // لو Jenkins على نفس السيرفر، لو لأ استبدلها بال IP
    }

    stages {
        stage('Checkout') {
            steps {
                // غير الرابط لرابط المشروع الخاص بيك
                git 'https://github.com/mostafa-7ussein/sonar-scanning-examples.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {  // الاسم اللي حطيته في Configure System
                    sh """
                    cd sonarqube-scanner-cli-simple
                    sonar-scanner \
                      -Dsonar.projectKey=${env.SONAR_PROJECT_KEY} \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=${env.SONAR_HOST_URL}
                    """
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
        success {
            echo "SonarQube analysis completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs."
        }
    }
}
