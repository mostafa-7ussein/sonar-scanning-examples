// pipeline {
//     agent any

//     environment {
//         SONAR_PROJECT_KEY = 'my-sample-project'   // غيره حسب مشروعك
//         SONAR_HOST_URL = 'http://localhost:5200'  // لو Jenkins و Sonar على نفس السيرفر
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 git 'https://github.com/mostafa-7ussein/sonar-scanning-examples.git'
//             }
//         }

//         stage('SonarQube Analysis') {
//             steps {
//                 withSonarQubeEnv('SonarQube') {  // تأكد ان اسمه مضبوط في Jenkins Configure System
//                     sh '''
//                     cd sonarqube-scanner-cli-simple
//                     sonar-scanner \
//                       -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
//                       -Dsonar.sources=. \
//                       -Dsonar.host.url=${SONAR_HOST_URL}
//                     '''
//                 }
//             }
//         }
//     }

//     post {
//         always {
//             echo "Pipeline finished."
//         }
//         success {
//             echo "SonarQube analysis completed successfully!"
//         }
//         failure {
//             echo "Pipeline failed. Check logs."
//         }
//     }
// }
pipeline {
    agent any

    environment {
        // اسم الـ SonarQube Server زي ما معرفته في Jenkins > Manage Jenkins > Configure System > SonarQube servers
        SONARQUBE_ENV = 'SonarQube'  
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(SONARQUBE_ENV) {
                    // شغل السونار باستخدام الأمر المناسب (مثلاً mvn sonar:sonar لو مشروع Maven)
                    sh 'mvn clean verify sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    // انتظر نتيجة الفحص من SonarQube
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}

