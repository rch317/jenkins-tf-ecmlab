node {
    stage('Checkout') {
        git branch: 'master', credentialsId: 'github', url: 'git@github.com:rch317/tf-ecmlab.git'
    }

    stage('Build') {
        sh 'echo "hello world"'
    }

    stage('Test') {
        sh 'echo "hello world: test"'
    }

    stage('Deploy') {
        sh 'echo "hello world: deploy"'
    }
}