pipeline{
    agent any
    environment {
        dockerHome = tool 'myDocker'
        mavenHome = tool 'myMaven'
        PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
    }

    stages{
        stage('checkout'){
            steps{
                h 'mvn --version'
				sh 'docker version'
				echo "Build"
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "BUILD_URL - $env.BUILD_URL"
            }
        }
        stage('Compile'){
            steps{
                sh "mvn clean compile"
            }
        }
        stage('Test'){
            steps{
                sh "mvn test"
            }
        }
        stage('Integreating Test'){
            steps{
                sh "mvn failsafe:integration-test failsafe:verify"
            }
        }
        stage('Package'){
            steps{
                sh "mvn package -DskipTests"
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                    dockerImage = docker.build("ksauto82/currency-exchange-devops:${env.BUILD_TAG}")
                }
            }
        }
        stage('Push Docker Image'){
            script{
                docker.withRegistry('','DockerHubID'){
                    dockerImage.push('latest')
                }
            }
        }
    }
    post{
        always {
            echo 'Im awesome. I run always'
        }
        success {
            echo 'I run when you are sucessful'
        }
        failure {
            echo 'I run when you fail'
        }
    }
}