node {

    // Mark the code checkout 'Checkout'....
    stage 'Checkout'

    // // Get some code from a GitHub repository
    git branch: 'master', url: 'git@github.com:rch317/tf-ecmlab.git'

    // Get the Terraform tool.
    def tfHome = tool name: 'Terraform', type: 'com.cloudbees.jenkins.plugins.customtools.CustomTool'
    env.PATH = "${tfHome}:${env.PATH}"
    wrap([$class: 'AnsiColorBuildWrapper', colorMapName: 'xterm']) {

            // Mark the code build 'plan'....
            stage name: 'Plan', concurrency: 1
            // Output Terraform version
            sh "terraform --version"
            //Remove the terraform state file so we always start from a clean state
            if (fileExists(".terraform/terraform.tfstate")) {
                sh "rm -rf .terraform/terraform.tfstate"
            }
            if (fileExists("status")) {
                sh "rm status"
            }
            sh "./init"
            sh "terraform get"
            sh "set +e; terraform plan -out=plan.out -detailed-exitcode; echo \$? > status"
            def exitCode = readFile('status').trim()
            def apply = false
            echo "Terraform Plan Exit Code: ${exitCode}"
            if (exitCode == "0") {
                currentBuild.result = 'SUCCESS'
                slackSend color: 'good', message: "No changes to be applied ${env.JOB_NAME} - ${env.BUILD_NUMBER} ()"
            }
            if (exitCode == "1") {
                slackSend color: '#0080ff', message: "Plan Failed: ${env.JOB_NAME} - ${env.BUILD_NUMBER} ()"
                currentBuild.result = 'FAILURE'
            }
            if (exitCode == "2") {
                stash name: "plan", includes: "plan.out"
                slackSend  color: 'good', message: "Plan Awaiting Approval: ${env.JOB_NAME} - ${env.BUILD_NUMBER} ()"
                try {
                    // input message: 'Apply Plan?', ok: 'Apply'
                    apply = true
                } catch (err) {
                   slackSend  color: 'warning', message: "Plan Discarded: ${env.JOB_NAME} - ${env.BUILD_NUMBER} ()"
                    apply = false
                    currentBuild.result = 'UNSTABLE'
                }
            }

            if (apply) {
                stage name: 'Apply', concurrency: 1
                unstash 'plan'
                if (fileExists("status.apply")) {
                    sh "rm status.apply"
                }
                sh 'set +e; terraform apply plan.out; echo \$? > status.apply'
                def applyExitCode = readFile('status.apply').trim()
                if (applyExitCode == "0") {
                   slackSend color: 'good', message: "Changes Applied ${env.JOB_NAME} - ${env.BUILD_NUMBER} ()"
                } else {
                   slackSend color: 'danger', message: "Apply Failed: ${env.JOB_NAME} - ${env.BUILD_NUMBER} ()"
                    currentBuild.result = 'FAILURE'
                }
            }
    }
}