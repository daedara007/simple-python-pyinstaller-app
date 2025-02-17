node {
    // Tahap build
    stage('Build') {
        docker.image('python:3.9').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            stash(name: 'compiled-results', includes: 'sources/*.py*')
        }
    }

    // Tahap test
    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        junit 'test-reports/results.xml'
    }

    // Tahap Persetujuan
    stage('Approval') {
        input message: 'Lanjut ke tahap deploy? (Klik "Proceed" untuk melanjutkan ke tahap Deploy)'
    }

    // Tahap Deploy
    stage('Deploy') {
        docker.image('python:3.9').inside('-u root') {
            sh 'pip install pyinstaller'
            sh 'pyinstaller --onefile sources/add2vals.py'
            echo 'Pipeline has finished successfully.'
        }
        archiveArtifacts 'dist/add2vals'
    }
}