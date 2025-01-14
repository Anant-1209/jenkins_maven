pipeline {
    agent any

    tools {
        maven 'sonarmaven' 
    }

    environment {
        SONARQUBE_SERVER = 'http://localhost:9000'  
        SONARQUBE_TOKEN = 'sqp_3d30084bdc7b0db36f7933312fcb323ac821b440'  
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your Git repository
                git 'https://github.com/Anant-1209/jenkins_maven.git' 
            }
        }

        stage('Build') {
            steps {
              
                sh 'mvn clean verify'  
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Run SonarQube analysis using the maven sonar plugin
                sh '''
                  mvn clean verify sonar:sonar \
                          -Dsonar.projectKey=mavensonarjenkins \
                          -Dsonar.projectName='mavensonarjenkins' \
                          -Dsonar.host.url=http://localhost:9000 \
                          -Dsonar.token=sqp_4319582a1e329049ff1aa4ead3d1ce025ca9438f
                '''
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    
                    def qualityGate = waitForQualityGate()
                    if (qualityGate.status != 'OK') {
                        error "Pipeline failed due to SonarQube Quality Gate failure!"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
