pipeline {
	agent none

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
			agent { label "UiPath"}
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
			agent { label "UiPath"}
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
			agent { label "UiPath"}
            steps {
                echo 'Testing..the workflow...'
            }
        }

         // Deploy Stages
        stage('Deploy to FAT') {
			agent { label "UiPath"}
            steps {
                echo "Deploying to FAT "

            }
        }
		
		stage('OK to deploy to PRD?') {
			agent none
			steps {
				input "Deploy to PRD?"
			}
		}
		
		
         // Deploy to Production Step
        stage('Deploy to Production') {
			agent { label "UiPath"}
            steps {
                echo 'Deploy to Production'
                }
            }
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