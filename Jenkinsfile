pipeline {
  agent any
  options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')
  }
   environment {
       service_name="${service_name}"
      //  TF_VAR_vpc_subnet_id="${vpc_subnet_id}"
      //  TF_VAR_vpc_id="${vpc_id}"
      //  TF_VAR_cifs_s3_bucket_suffix="${DATASTORE}"
      //  TF_VAR_gateway_base_ami="${NETWORK}"
      //  TF_VAR_gateway_name= "${gateway}"
    }
    parameters {[
      [$class: 'ChoiceParameter', 
          choiceType: 'PT_SINGLE_SELECT', 
          description: 'Select the Service Name from the Dropdown List', 
          filterLength: 1, 
          filterable: false, 
          name: 'Service_Name', 
          script: [
              $class: 'GroovyScript', 
              fallbackScript: [
                  classpath: [], 
                  sandbox: false, 
                  script: 
                      "return['Could not get the services']"
              ], 
              script: [
                  classpath: [], 
                  sandbox: false, 
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
                      sandbox: false, 
                      script: "return['Could not get Service from Service Param']"
                      ], 
              script: [
                      classpath: [], 
                      sandbox: false, 
                      script: '''
                      if(resource_type.equals("EC2")) 
                          { return ["Instances", "Volumes", "AMI"] } 
                      else if(resource_type.equals("SSM")) 
                          { return ["Run Command", "Automation", "Document"] } 
                      else
                          { return ["Empty"] } 
                      '''
                  ] 
          ]
      ]
    ]

        // choice(name: 'resource_type', choices: ['EC2', 'RDS', 'S3', 'SSM'],)
        // string(name: 'resource_name', defaultValue: 'ec2', description: 'give resource name',)
        // activeChoiceReactiveParam('service_name') {
        //     description('select your choice')
        //     choiceType('RADIO')
        //     groovyScript {
        //         script('''
        //         if(resource_type.equals("EC2")) 
        //           { return ["Instances", "Volumes", "AMI"] } 
        //         else if(resource_type.equals("SSM")) 
        //           { return ["Run Command", "Automation", "Document"] } 
        //         else
        //           { return ["Empty"] } 
        //          ''')
        //         fallbackScript('return ["error"]')
        //     }
        //     referencedParameter('resource_type')
        // }

        // choice(name: 'resource_type', script{if (resource_name == "EC2") {
        //   return ["Instances", "Load Balancers", "Security Groups"]
        // } else if (resource_name == "SSM") {
        //   return ["Run Command", "Documents"]
        // }})
        // string(name: 'resource_type', defaultValue: 'instances', description: 'select resource type',)
        
    }
  stages {
    stage('Initial') {
          steps {
            script {
              try {
                    sh '''
                        echo $service_name
                        python3 --version
                        python3 Hello.py
                     '''
                } catch (err) {
                       echo "Caught: ${err}"
                       currentBuild.result = 'FAILURE'
                  }
            }
      }
    }

  }
}
