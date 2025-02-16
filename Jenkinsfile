node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    
    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
    }
    
    stage('Post-Test') {
        junit 'test-reports/results.xml'
    }
    
    stage('Deliver') {
        docker.image('python:2').inside {
            sh 'pip install pyinstaller'
            sh 'pyinstaller --onefile --noconfirm sources/add2vals.py'
        }

    }
    
    stage('Post-Deliver') {
        archiveArtifacts 'dist/add2vals'
    }
}
