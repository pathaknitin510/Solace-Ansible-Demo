def approvalMap             // collect data from approval step

pipeline {
    agent none

    stages {
        stage('Stage 1') {
            agent none
            steps {
              mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', 
		      charset: 'UTF-8', from: 'pathaknitin510@gmail.com', mimeType: 'text/html', replyTo: '', subject: "Approval Pending: Project name -> ${env.JOB_NAME}", to: "nitinpathak.orai@gmail.com";  
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
        stage('Checkout Scm') {
         steps {
           git(credentialsId: '77762aec-11e0-4ff4-91a3-8e9a299ead22', url: 'https://github.com/pathaknitin510/solace-accelerator-demo')
      }
    }

        stage('Shell script 0') {
         steps {
           sh 'ansible-playbook solace-vpn.yml $Tags $Tasks -i $Environment'
         }
      }
	    
    }
} 				
