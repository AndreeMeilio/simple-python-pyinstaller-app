node {
    checkout scm
    stage("Build"){
        docker.image('python:3').withRun("-v .:/app"){ c -> 
            sh 'python2 -m py_compile /app/sources/add2vals.py /app/sources/calc.py'
        }
    }
    stage("Test"){
        docker.image('qnib/pytest').withRun("-v .:/app"){ c -> 
            sh 'py.test --verbose --junit-xml /app/test-reports/results.xml /app/sources/test_calc.py'
        }
        junit 'test-reports/results.xml'
    }
}