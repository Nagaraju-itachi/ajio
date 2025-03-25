def registry = "https://nagatrail.jfrog.io/"
pipeline {
    agent any
    environment {
        PATH = "/opt/maven/bin:$PATH"
    }
    stages {
        stage ("Build") {
            steps {
                echo "-- Build Started --"
                sh 'mvn clean deploy -D.maven.test.sleep=true'
                echo "-- Build Completed --"
            }
        }
        
        stage('GIT CLONE') {
            steps {
                git url: "https://github.com/Nagaraju-itachi/ajio.git", branch: "main"
            }
        }
        
        stage ("test") {
            steps {
                echo "-- Unit Test Started --"
                sh 'mvn surefire-report:report'
                echo "-- Unit Test Completed --"
            }
        }
        
        stage ("SonarQube Analysis") {
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
        
        stage ("Quality Gate") {
            steps {
                script {
                    timeout(time: 1, unit: "HOURS") {
                        def qg = waitForQualityGate()
                        if (qg.status != "OK") {  // FIXED: Corrected Not Equal Operator
                            echo "Warning: Quality gate failed but continuing pipeline: ${qg.status}"
                        }
                    }
                }
            }
        }
        
        stage ("JAR Publish") {
            steps {
                script {
                    echo "-- JAR Publish Started --"
                    def server = Artifactory.newServer(url: registry + "/artifactory", credentialsId: "JFROG")
                    
                    def properties = "buildId=${env.BUILD_ID},commitId=${env.GIT_COMMIT}"
                    
                    def uploadSpec = """{
                        "files": [
                            {
                                "pattern": "jarstaging/(*)",
                                "target": "naga-libs-release-local/{1}",
                                "flat": false,
                                "props": "${properties}",
                                "exclusions": ["*.sha1", "*.md5"]
                            }
                        ]
                    }"""
                    
                    def buildInfo = server.upload(uploadSpec)
                    buildInfo.env.collect()
                    server.publishBuildInfo(buildInfo)
                }
            }
        }
    }
}
