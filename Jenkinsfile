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
				echo " -- Build started -- "
                sh 'mvn clean deploy -D maven.test.sleep=true'
				echo " -- Build Completed -- "
            }
        }
		
		stage('Test') {
            steps {
				echo " -- Unit Test started -- "
                sh 'mvn surefire-report:report'
				echo " -- Unit Test Completed -- "
            }
        }

        stage('SonarQube Analysis') {
		environment {
			SONAR_SCANNER_HOME = tool "Nagaraju-itachi-sonar-scanner"
		}
			steps {
				withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
					withSonarQubeEnv('Nagaraju-itachi-Sonar-server') {
					sh """
						${SONAR_SCANNER_HOME}/bin/sonar-scanner \
						-Dsonar.verbose=true \
						-Dsonar.organization=nagarajusonar \
						-Dsonar.projectKey=nagarajusonar_nagarajutrend \
						-Dsonar.projectName=nagarajusonar \
						-Dsonar.sourceEncoding=UTF-8 \
						-Dsonar.sources=. \
						-Dsonar.java.binaries=target/classes \
						-Dsonar.host.url=https://sonarcloud.io \
						-Dsonar.login=${SONAR_TOKEN}
						"""
					}
				}

			}
		}
	}
}
