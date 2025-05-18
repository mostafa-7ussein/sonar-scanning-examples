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

    tools {
        // اسم الـ JDK المثبت في Jenkins
        jdk 'JDK11'
        // اسم أداة Maven لو بتستخدمها
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                // سحب الكود من Git
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // بناء المشروع (مثلاً باستخدام Maven)
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // استخدام SonarQube Server المسجل في Jenkins باسم 'SonarQube'
                withSonarQubeEnv('SonarQube') {
                    // أمر تشغيل Sonar Scanner، تأكد أنه مثبت في بيئة Jenkins أو مشروعك
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // انتظار نتيجة جودة الكود من SonarQube
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}

