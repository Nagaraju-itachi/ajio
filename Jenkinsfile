pipeline {
	agent any
	environment {
	PATH= "/opt/maven/bin:$PATH"
	}
	stages {
		stage ('GIT CLONE') {
			steps {
				git url:"https://github.com/Nagaraju-itachi/ajio.git" , branch:"main"
			}
		}
		stage ('Build'){
			steps {
				sh 'mvn clean package'
			}
		}
	}
}
