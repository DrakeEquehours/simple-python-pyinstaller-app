node {
    stage('Build') {
        // Build stage inside a Docker container
        withDockerContainer('python:2-alpine') {
            checkout scm
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    
    stage('Test') {
        // Test stage inside a Docker container
        withDockerContainer('qnib/pytest') {
            checkout scm
            sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            junit 'test-reports/results.xml'
        }
    }
    
    stage('Deploy') {
        // Deploy stage with an interactive Docker container
        sh '''
            docker run --rm -it --entrypoint=/bin/bash cdrx/pyinstaller-linux:python2 -c "
            git clone ${env.GIT_URL} repo && cd repo &&
            pyinstaller --onefile sources/add2vals.py"
        '''
    }
}
