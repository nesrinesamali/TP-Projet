pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'       // Nom exact dans Jenkins
        maven 'M2_Home'       // Nom exact dans Jenkins
    }

    environment {
        SONARQUBE = 'sonarqube1'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/nesrinesamali/TP-Projet.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(SONARQUBE) {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    script {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Quality gate failed: ${qg.status}"
                        }
                    }
                }
            }
        }
    }
}
