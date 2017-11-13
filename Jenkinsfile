node {
    stage('Checkout') {
        git branch: 'master', credentialsId: '9d7f8c3b-0bbd-4a91-80d7-4e89155c0dd1', url: 'git@github.com:rch317/tf-ecmlab.git'
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