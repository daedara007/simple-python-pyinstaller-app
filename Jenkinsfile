node {
    // Use docker in each stage
    stage('Build') {
        docker.image('docker.io/python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    
    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        // Archiving results after the test stage
        junit 'test-reports/results.xml'
    }
    
    stage('Deliver') {
        docker.image('cdrx/pyinstaller-linux:python2').inside {
            sh 'pyinstaller --onefile sources/add2vals.py'
        }
        // Archive artifacts if the build is successful
        archiveArtifacts 'dist/add2vals'
    }
}
