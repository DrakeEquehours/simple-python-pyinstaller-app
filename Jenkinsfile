node {
    stage('Build') {
        withDockerContainer('python:2-alpine') {
            checkout scm
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    
    stage('Test') {
        withDockerContainer('qnib/pytest') {
            checkout scm
            sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            junit 'test-reports/results.xml'
        }
    }
    
    stage('Deploy') {
        sh '''
            docker run --rm -v ${WORKSPACE}:/workspace -w /workspace cdrx/pyinstaller-linux:python2 /bin/bash -c "
            pyinstaller --onefile sources/add2vals.py"
        '''

        archiveArtifacts artifacts: 'dist/add2vals', allowEmptyArchive: true
    }
}
