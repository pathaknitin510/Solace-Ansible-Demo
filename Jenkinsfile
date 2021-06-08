def approvalMap             // collect data from approval step

pipeline {
    agent none

    stages {
        stage('Stage 1') {
            agent none
            steps {
             
                timeout(60) {                // timeout waiting for input after 60 minutes
                    script {
                        // capture the approval details in approvalMap. 
                         approvalMap = input( 
                                        id: 'test', 
										message: 'Hello', 
                                        ok: 'Proceed?', 
                                        parameters: [
                                            choice(
                                                choices: 'Dev-env.ini\nTest-env.ini', 
                                                description: 'Select an Env for this build', 
                                                name: 'Environment'
                                            ),
                                        string(
                                                defaultValue: '', 
                                                description: '', 
                                                name: 'Task'
                                            )
                                        ], 
                                        submitter: 'opsApprover', 
                                        submitterParameter: 'APPROVER')	
                            }
                }
            }
        }
        stage('Stage 2') {
            agent any

            steps {
                // print the details gathered from the approval
                echo "This build was approved by: ${approvalMap['APPROVER']}"  	
                echo "This build is made using Env: ${approvalMap['Environment']}"
                echo "This build is using Task: ${approvalMap['Task']}"
            }
        }
    }
} 				
