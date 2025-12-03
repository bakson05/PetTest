pipeline {
    agent any

    tools {
        maven 'Maven3'   // Le nom de ton Maven dans Jenkins (Manage Jenkins → Global Tool Configuration)
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/bakson05/PetTest.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token')  // ID du token dans Jenkins
            }
            steps {
                withSonarQubeEnv('SonarQube_Server') {
                    sh """
                        mvn sonar:sonar \
                          -Dsonar.projectKey=users-service \
                          -Dsonar.host.url=http://192.168.1.130:9000 \
                          -Dsonar.login=$SONAR_TOKEN
                    """
                }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
