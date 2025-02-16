pipeline {
    agent {
        docker { image 'python:3.9' }
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Setting up Python environment...'
                    sh 'python3 -m venv venv'
                    sh '. venv/bin/activate'
                    sh 'pip install --upgrade pip'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running unit tests...'
                    sh '. venv/bin/activate && python3 -m unittest sources/test_calc.py'
                }
            }
        }

        stage('Deliver') {
            steps {
                script {
                    echo 'Running add2vals.py as a final check...'
                    sh '. venv/bin/activate && python3 sources/add2vals.py 10 20'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build, Test, and Deliver stages completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed! Check logs for errors.'
        }
    }
}
