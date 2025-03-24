pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    tools {
        maven 'Maven' // Make sure Maven is configured under Jenkins tools
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Nagaraju-itachi/ajio.git', branch: 'main'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeCloud') {
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
