pipeline {
  agent any
  options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')
  }
   environment {
       resourceName="${resource_name}"
      //  TF_VAR_vpc_subnet_id="${vpc_subnet_id}"
      //  TF_VAR_vpc_id="${vpc_id}"
      //  TF_VAR_cifs_s3_bucket_suffix="${DATASTORE}"
      //  TF_VAR_gateway_base_ami="${NETWORK}"
      //  TF_VAR_gateway_name= "${gateway}"
    }
    parameters {
        choice(name: 'resource_name',choices: ['EC2', 'RDS', 'S3', 'SSM'],)
        // string(name: 'resource_name', defaultValue: 'ec2', description: 'give resource name',)
        
        choice(name: 'resource_type',choices: script{if (resource_name == "EC2") {
          return ["Instances", "Load Balancers", "Security Groups"]
        } else if (resource_name == "SSM") {
          return ["Run Command", "Documents"]
        }})
        // string(name: 'resource_type', defaultValue: 'instances', description: 'select resource type',)
        
    }
  stages {
    stage('Terraform init') {
      when { branch 'master' }
          steps {
            script {
              try {
                    sh '''
                        echo $resource_name
                        echo python --version
                        python Hello.py
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