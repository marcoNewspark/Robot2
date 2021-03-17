pipeline {
    agent { label "UiPath"}

        // Environment Variables
        environment {
        
        //Orchestrator Services
        UIPATH_ORCH_URL = "https://cloud.uipath.com/"
        UIPATH_ORCH_LOGICAL_NAME = "newspark"
        UIPATH_ORCH_TENANT_NAME = "Default"
        UIPATH_ORCH_FOLDER_NAME = "Robot2"
    }

    stages {

        // Printing Basic Information
        stage('Preparing'){
            steps {
                echo "Jenkins Home ${env.JENKINS_HOME}"
                echo "Jenkins URL ${env.JENKINS_URL}"
                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
                echo "Jenkins JOB Name ${env.JOB_NAME}"
                echo "GitHub BranhName ${env.BRANCH_NAME}"
                checkout scm

            }
        }

         // Build Stages
        stage('Build') {
            steps {
                echo "Building..with ${WORKSPACE}"
                UiPathPack (
                      outputPath: "Output\\${env.BUILD_NUMBER}",
                      projectJsonPath: "project.json",
                      version: AutoVersion(),
                      useOrchestrator: false
        )
            }
        }
         // Test Stages
        stage('Test') {
            steps {
                echo 'Testing..the workflow...'
            }
        }

         // Deploy Stages
        stage('Deploy to FAT') {
            steps {
                echo "Deploying to FAT "

            }
        }
		
		stage('OK to deploy to UAT?') {
			steps {
				input "Deploy to UAT?"
			}
		}
		
		
         // Deploy to Production Step
        stage('Deploy to Production') {
            steps {
                echo 'Deploy to Production'
                }
            }
    }

    // Options
    options {
        // Timeout for pipeline
        timeout(time:80, unit:'MINUTES')
        skipDefaultCheckout()
    }


    // 
    post {
        success {
            echo 'Deployment has been completed!'
        }
        failure {
          echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
        }
        
    }

}