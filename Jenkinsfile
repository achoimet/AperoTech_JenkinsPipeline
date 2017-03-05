pipeline {
    agent any
    stages {
        stage('Unit Test') {
            steps {
                echo 'Run tests'
				sleep 5
            }
        }
		
		
		stage('Deploy') {
            steps {
                echo 'Deploy'
				sleep 3
            }
        }
		
		stage('QA Tests') {
            steps {
                echo 'Running automated tests'
				sleep 3
            }
        }
		
		stage('Prod') {
            steps {
                echo 'Deploy to prod'
				sleep 3
            }
        }
    }
    post { 
        always { 
            echo 'Tests results sent to JIRA!'
        }
    }
}