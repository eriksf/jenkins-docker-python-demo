node {
    stage('Source') {
        echo 'This is build ${env.BUILD_TAG} '
        sh 'printenv'
    }
    stage('Cleanup') {
        step([$class: 'WsCleanup'])
    }
    stage('Checkout SCM') {
        checkout scm
    }
    def pythonImage
    stage('build docker image') {
        pythonImage = docker.build("maxsum:build")
    }
    stage('test') {
        pythonImage.inside {
            sh 'id && . /tmp/venv/bin/activate && python -m pytest --junitxml=build/results.xml'
        }
    }
    stage('collect test results') {
        junit 'build/results.xml'
    }
}
