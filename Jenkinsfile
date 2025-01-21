node {
    checkout scm
    stage("Build"){
        docker.image('python:2-alpine').withRun("-v ${env.WORKSPACE}:${env.WORKSPACE} -w ${env.WORKSPACE}"){ c -> 
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage("Test"){
        docker.image('qnib/pytest').withRun("-v ${env.WORKSPACE}:${env.WORKSPACE} -w ${env.WORKSPACE}"){ c -> 
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        junit 'test-reports/results.xml'
    }
}