properties([ 
    parameters([
        [$class: 'ChoiceParameter', 
            choiceType: 'PT_SINGLE_SELECT', 
            description: 'Select the Service Name from the Dropdown List',
            name: 'Service_Name', 
            script: [
                $class: 'GroovyScript', 
                fallbackScript: [
                    classpath: [], 
                    sandbox: true, 
                    script: 
                        "return['Could not get the services']"
                ], 
                script: [
                    classpath: [], 
                    sandbox: true, 
                    script: 
                        "return['EC2','SSM','RDS']"
                ]
            ]
        ],
        [$class: 'CascadeChoiceParameter', 
            choiceType: 'PT_SINGLE_SELECT', 
            description: 'Select the service type from the Dropdown List',
            name: 'Service_Type', 
            referencedParameters: 'Service_Name', 
            script: 
                [$class: 'GroovyScript', 
                fallbackScript: [
                        classpath: [], 
                        sandbox: true, 
                        script: "return['Could not get Service from Service Param']"
                        ], 
                script: [
                        classpath: [], 
                        sandbox: true, 
                        script: '''
                        if(Service_Name.equals("EC2")) 
                            { return ["Instances", "Volumes", "AMI"] } 
                        else if(Service_Name.equals("SSM")) 
                            { return ["Run Command", "Automation", "Document"] } 
                        else
                            { return ["Empty"] } 
                        '''
                    ] 
            ]
        ]
    ])
])
pipeline {
  agent any
  options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')
  }
   environment {
      resource_type="Hello"
    }

  stages {
    stage('Initial') {
          steps {
            script {
                def $vars=params.Service_Name
                try {
                    sh '''
                        #!/bin/bash
                        echo ${vars}
                        echo $resource_type
                        echo "Hello"
                     '''
                        // python3 --version
                        // python3 Hello.py
                } catch (err) {
                       echo "Caught: ${err}"
                       currentBuild.result = 'FAILURE'
                  }
            }
      }
    }

  }
}
