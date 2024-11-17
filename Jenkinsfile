pipeline{
    agent{
        node{
            label 'AGENT-1'
        }
    }
    // environment{
    //     packageVersion = ''
    //     nexusUrl = 'ip-address:port-number'
    // }
    options{
        ansiColor('xterm')
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
    }
    parameters{
        string(name: 'version', defaultValue: '1.0.0', description: 'What is the artifact Version?')
        string(name: 'environment', defaultValue: 'dev', description: 'What is the environment?')
    }

    stages{
        stage('Print Version'){
            steps{
                sh """
                    echo "version: ${params.version}"
                    echo "environment: ${params.environment}"
                """
            }
        }
        stage('Init'){
            steps{
                sh """
                    cd terraform
                    terrafrom init --backend-config=${params.environment}/backend.tf -reconfigure
                """
            }
        }
        stage('Plan'){
            steps{
                sh """
                    cd terraform
                    terrafrom plan -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}"
                """
            }
        }
        stage('Apply'){
            steps{
                sh """
                    cd terraform
                    terrafrom apply -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}" -auto-approve
                """
            }
        }


    post{
        always{
            deleteDir()
        }
    }
}