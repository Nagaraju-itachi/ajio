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
			SONAR_SCANNER_HOME = tool "Nagaraju-itachi-sonar-scanner"
			SONAR_TOKEN = "864f20bee125380caa9956d1238aea9cd846ab88"
		}
			steps {
				withSonarQubeEnv('Nagaraju-itachi-Sonar-server') {
					sh """
						${SONAR_SCANNER_HOME}/bin/sonar-scanner
      						/var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/Nagaraju-itachi-sonar-scanner/bin/sonar-scanner
						-Dsonar.verbose=true
						-Dsonar.organization=nagarajusonar
						-Dsonar.projectKey=nagarajusonar_nagarajutrend
						-Dsonar.projectName=nagarajusonar
						-Dsonar.language=java
						-Dsonar.sourceEncoding=UTF-8
						-Dsonar.sources=.
						-Dsonar.java.binaries=target/classes
						-Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml
						-Dsonar.host.url=https://sonarcloud.io
						-Dsonar.login=${SONAR_TOKEN}
						"""
				}
			}
		}
	}
}
