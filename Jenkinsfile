pipeline {
    agent any

    environment {
        MAVEN_HOME = "/opt/maven"
        PATH = "${MAVEN_HOME}/bin:${env.PATH}"
    }

    stages {

        stage("Build") {
            steps {
                echo "------- Build started --------"
                sh 'mvn clean package'
                echo "------- Build completed --------"
            }
        }

        stage("Test") {
            steps {
                echo "------- Unit test started --------"
                sh 'mvn test'
                sh 'mvn surefire-report:report'
                echo "------- Unit test completed --------"
            }
        }

        stage("SonarQube Analysis") {
            environment {
                scannerHome = tool 'rahimdemy-sonar-scanner'
            }
            steps {
                withSonarQubeEnv('rahimdemy-sonarqube-server') {
                    sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=irctc \
                        -Dsonar.sources=src \
                        -Dsonar.java.binaries=target
                    """
                }
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}

