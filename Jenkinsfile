pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
    }


    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Nagaraju-itachi/ajio.git', branch: 'main'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('Nagaraju-itachi-Sonar') {
                    sh 'mvn clean verify sonar:sonar'
                }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }
    }
}
