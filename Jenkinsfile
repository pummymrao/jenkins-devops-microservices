pipeline {
	agent any
	// agent { docker {image "node:latest"}}
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages {
		stage('Checkout') {
			steps {
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
				echo "$PATH"
				echo "Build Number - $env.BUILD_NUMBER"
				echo "Build ID - $env.BUILD_ID"
				echo "Job Name - $env.JOB_NAME"
				echo "BUILD TAG - $env.BUILD_TAG"
				echo "BUILD URL - $env.BUILD_URL"
		}
		}
		stage('Compile') {
			steps {
				sh "mvn clean compile"
			}
		}
		// stage('Test') {
		// 	steps {
		// 		sh "mvn test"
		// 	}
		// }
		// stage('Integration Test') {
		// 	steps {
		// 		sh "mvn failsafe:integration-test failsafe:verify"
		// 	}
		// }
		stage('Package') {
			steps {
				sh "mvn package -DskipTests"
			}
		}
		stage('Docker Build') {
			steps {
				script {
					dockerImage = docker.build("pramodmra/currency-exchange-devops:{$env.BUILD_TAG}")
					
				}
			}
		}
		stage('Docker Push') {
			steps {
				script {
					docker.withRegistry('', 'a6dea94e-5c4b-4180-a76b-c4e1f74819d3') {
						dockerImage.push()
						dockerImage.push("latest")
					}
				}
			}
		}
}
post {
	always {
		echo "Always"
	}
	success {
		echo "Success"
	}
	failure {
		echo "Failure"
	}
}
}