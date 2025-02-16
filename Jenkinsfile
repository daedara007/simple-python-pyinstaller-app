node {
    stage('Build') {
        echo 'Installing dependencies...'
        sh 'pip3 install -r requirements.txt || echo "No requirements.txt found, skipping..."'
        sh 'python3 -m py_compile sources/add2vals.py sources/calc.py'
    }

    stage('Test') {
        echo 'Running tests...'
        sh 'python3 -m unittest sources/test_calc.py'
    }

    stage('Deliver') {
        echo 'Archiving scripts...'
        archiveArtifacts artifacts: '**/*.py', fingerprint: true
        echo 'Delivery complete.'
    }
}