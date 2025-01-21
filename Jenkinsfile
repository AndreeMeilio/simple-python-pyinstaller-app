node {
    checkout scm
    stage("Build"){
        docker.image('python:2-alpine').withRun("-v ${WORKSPACE}:/app"){ c -> 
            sh 'python -m py_compile /app/sources/add2vals.py sources/calc.py'
        }
    }
    stage("Test"){
        docker.image('qnib/pytest').withRun("-v ${WORKSPACE}:/app"){ c -> 
            sh 'py.test --verbose --junit-xml /app/test-reports/results.xml sources/test_calc.py'
        }
        junit 'test-reports/results.xml'
    }
}