// stage('build') {
//     steps {
//         sh 'python Hello.py'
//     }
// }
pipeline {
  agent any
  parameters {
    string(name: 'TICKET_NUMBER', description: 'Get a ticket Number')
    string(name: 'Execution_Type', description: 'Add the execution type')
  }
  options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '100')
  }
  stages { stage('Cross-account access') { steps { script {

    if (params.Execution_Type == "serverProvisioning") {
        // Put your AWS code here
        sh """#!/bin/bash
        python --version
        """
        // python TemplateScripts/ServerProvisioning/ServerPreDeployment/ec2_deploy.py ${params.TICKET_NUMBER}
        // python TemplateScripts/ServerProvisioning/ServerPostDeployment/postDeployment.py
    }
    if (params.Execution_Type == "serverTermination") {
        // Put your AWS code here
        sh """#!/bin/bash
        python --version
        """
        // python TemplateScripts/ServerDecommission/ServerDecommission.py
    }
    
  
  }}}}
}