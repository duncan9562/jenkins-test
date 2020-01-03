pipeline {
    agent{
        docker{
              label 'docker && new'
              image 'hashicorp/terraform:0.12.18'
              args '-u 1000:1000 --entrypoint ""'
              reuseNode true
          }
          environment{
              TF_IN_AUTOMATION = 1
          }
          options {
              timeout(time: 1, unit: "HOURS")
              buildDiscarder(logRotator(numToKeepStr: "5"))
          }
    }
    stages {
        stage("Formatting") {
            steps {
                sh "terraform fmt -list=true -check=true"
            }
        }
        stage("Initialise") {
            steps {
                sh "terraform init -input=false"
            }
        }
    }
    post{
        cleanup {
            script {
                currentBuild.result = currentBuild.result ?: 'SUCCESS'
                notifyBitbucket()
            }
            deleteDir()
        }
    }    
}
