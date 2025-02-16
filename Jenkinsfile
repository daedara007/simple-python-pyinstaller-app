node {
    stage('Build') {
        echo 'Installing dependencies...'
        sh 'pip install -r requirements.txt || echo "No requirements.txt found, skipping..."'
    }

    stage('Test') {
        echo 'Running tests...'
        sh 'python3 -m unittest test_calc.py'
    }

    stage('Deliver') {
        echo 'Archiving scripts...'
        archiveArtifacts artifacts: '**/*.py', fingerprint: true
        echo 'Delivery complete.'
    }
}