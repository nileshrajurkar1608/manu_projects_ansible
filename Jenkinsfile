pipeline {
    agent any    
    environment {
        ENV_URL       = "pipeline.google.com"
        SSH_CRED      = credentials('SSH')
    }

    stages {
        stage('Perform Lint Checks') {    // Runs only when it's a feature branch 
        when { branch pattern: "feature-.*", comparator: "REGEXP"} 
            steps {
                sh "env"
                sh "echo Performing Link Checks"           
            }
        }

        stage('Do a Dry-Run') {  
            when { branch pattern: "PR-.*", comparator: "REGEXP"}           // Runs only when it's a PR 
            steps {
                sh "env"
                sh "ansible-playbook robot-dryrun.yml -e COMPONENT=mongodb -e ENV=dev -e ansible_user=${SSH_CRED_USR} -e ansible_password=${SSH_CRED_PSW}"           
            }
        }

        stage('TAG') {   
            when { 
                expression { env.TAG_NAME != null }        // Checking whether the value of TAG_NAME is null or not
                }    
            steps {
                sh "echo I am running against a TAG"                
            }
        }
    }
}


// TAG_NAME variable only comes up when you run against the TAG_NAME, rest of the times, it won't be there. 