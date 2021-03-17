pipeline {
	agent none

        // Environment Variables
        environment {
		
		//Robot Package name
        PACKAGENAME = "Robot2"
        //Orchestrator Services
        UIPATH_ORCH_URL = "https://cloud.uipath.com/"
        UIPATH_ORCH_LOGICAL_NAME = "newspark"
		
		//tenant and folder
        UIPATH_ORCH_TENANT_NAME = "Default"
        UIPATH_ORCH_FOLDER_NAME = "Robot2"
			
		
		PACKAGEPATH = "\\\\192.168.1.3\\Repository\\${env.PACKAGENAME}\\${env.BUILD_NUMBER}"
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
				echo "Repository path ${env.PACKAGEPATH}"
                checkout scm

            }
        }

         // Build Stages
        stage('Build') {
			agent { label "UiPath"}
            steps {
                echo "Building..with ${WORKSPACE}"
                UiPathPack (
                      outputPath: "${env.PACKAGEPATH}",
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
		
		//stage('OK to deploy to PRD?') {
//			agent none
			//steps {
//				input "Deploy to PRD?"
			//}
		//}
		
		
         // Deploy to Production Step
        stage('Deploy to Production') {
			agent { label "UiPath"}
            steps {
                echo 'Deploy to Production'
				UiPathDeploy(
					credentials: Token(accountName: "Newspark", credentialsId: "UiPathAPI"), 
					environments: "", 
					folderName: "Robot2", 
					orchestratorAddress: "https://cloud.uipath.com/", 
					orchestratorTenant: "Default", 
					packagePath: "${env.PACKAGEPATH}" 
				)
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