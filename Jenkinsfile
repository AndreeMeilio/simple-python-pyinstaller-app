node {
    checkout scm
    stage("Build"){
        docker.image("python:2.7").inside('-v ${env.WORKSPACE}:${env.WORKSPACE} -w ${env.WORKSPACE}'){
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage("Test"){
        docker.image("qnib/pytest").inside('-v ${env.WORKSPACE}:${env.WORKSPACE} -w ${env.WORKSPACE}'){
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
}