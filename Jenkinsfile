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
		environment {
			scannerHome = tool "Nagaraju-itachi-sonar-scanner"
		}
			steps {
				withSonarQubeEnv('Nagaraju-itachi-Sonar-server') {
				
				sh "${scannerHome}/bin/sonar-scanner"
				}
			}
		}
	}
}
