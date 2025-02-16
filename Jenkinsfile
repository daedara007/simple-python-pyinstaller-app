pipeline {
    agent {
        docker { image 'python:3.9' }
    }

    stages {
        stage('Build') {
                    agent {
                        docker {
                            image 'python:2-alpine'
                        }
                    }
                    steps {
                        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                    }
        }

        stage('Test') {
                    agent {
                        docker {
                            image 'qnib/pytest'
                        }
                    }
                    steps {
                        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
                    }
                    post {
                        always {
                            junit 'test-reports/results.xml'
                        }
                    }
                }

        stage('Deliver') {
            steps {
                script {
                    echo 'Running add2vals.py as a final check...'
                    sh 'python3 sources/add2vals.py 10 20'
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
