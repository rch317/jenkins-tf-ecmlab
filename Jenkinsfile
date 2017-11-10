node {
    stage('Checkout') {
        git branch: 'master', 
          credentialsId: 'git_ssh', 
          url: 'ssh://git@github.com:rch317/tf-ecmlab.git'
    }

    stage('Build') {
        //slackSend channel: 'jenkins-notify', color: 'good', message: "Building ${env.JOB_NAME} - ${env.BUILD_NUMBER} (${env.BUILD_URL})  ${env.BUILD_TAG}"
        // Start the build
        sh 'echo "hello world"'
    }

    stage('Test') {
        // Perform some testing
        sh 'echo "hello world: test"'
    }

    stage('Deploy') {
        // Deploy the build
        sh 'echo "hello world: deploy"'
    }
}