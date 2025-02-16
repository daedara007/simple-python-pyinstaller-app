node {
    stage('Build') {
        docker.image('python:3.9').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            stash(name: 'compiled-results', includes: 'sources/*.py*')
        }
    }

    stage('Test') {
        docker.image('qnib/pytest').inside {
            catchError(buildResult: 'FAILURE', stageResult: 'UNSTABLE') {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
        }
        post {
            always {
                script {
                        junit 'test-reports/results.xml'
                    }  
                    }
                }
            }
        }
    }

    stage('Approval') {
        node {
            input message: 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk melanjutkan ke tahap Deploy)'
        }
    }

    stage('Deploy') {
        docker.image('python:3.9').inside('-u root') {
            sh 'pip install pyinstaller'
            sh 'pyinstaller --onefile sources/add2vals.py'
            sleep 60 // 1 minute
            echo 'Pipeline has finished successfully.'
        }
        archiveArtifacts 'dist/add2vals'
    }
}
