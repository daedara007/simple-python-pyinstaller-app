node {
    def server = "root@192.168.1.200"  // Ganti dengan username dan IP server
    def remotePath = "/opt/deploy"      // Ganti dengan path tujuan di server

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

    // Tahap Deploy lokal
    stage('Deploy') {
        docker.image('python:3.9').inside('-u root') {
            sh 'pip install pyinstaller'
            sh 'pyinstaller --onefile sources/add2vals.py'
            echo 'Pipeline has finished successfully.'
        }
        archiveArtifacts 'dist/add2vals'
    }

    // Tahap Deploy ke server
    stage('Deploy to Server') {
                sh "scp dist/add2vals ${server}:${remotePath}/add2vals"
                sh "ssh ${server} 'chmod +x ${remotePath}/add2vals && ${remotePath}/add2vals'"
                echo 'Pipeline has finished successfully.'
    }
}