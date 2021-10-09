pipeline {
    //agent any
    agent { label 'jenkins-agent' }
    
    environment {
    	dockerImage = ''
    }
    
    stages {
        stage('Build Image') {
            steps {
                script {
                        dockerImage = docker.build("ashkumarkdocker/docker-e2e-automation")
                }
            }
        }
        stage('API Automation') {
        	agent {
                docker {
                    image 'ashkumarkdocker/docker-e2e-automation'
                    reuseNode true // Run the container on the node specified at the top-level of the Pipeline, in the same workspace, rather than on a new node entirely
                    args '-v $HOME/.m2:/root/.m2'
                }
            }
            steps {
            	sh 'mvn test -Dcucumber.filter.tags="@API"'
            }
        }
        stage('UI Automation') {
        	agent {
                docker {
                    image 'ashkumarkdocker/docker-e2e-automation'
                    reuseNode true
                    args '-v $HOME/.m2:/root/.m2'
                }
            }
            steps {
            	sh 'mvn test -Dcucumber.filter.tags="@UI"'
            }       
        }
	}
}