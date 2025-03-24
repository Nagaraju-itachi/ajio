pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    stages {
        stage('GIT CLONE') {
            steps {
                git url: "https://github.com/Nagaraju-itachi/ajio.git", branch: "main"
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('Nagaraju-itachi-Sonar') {
                    def scannerHome = tool 'nagaraju-itachi-sonar-scanner'
                    sh "${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=nagaraju-itachi \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target"
                }
            }
        }
    }
}
