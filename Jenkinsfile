pipeline {
    agent {
        label 'agent-2-label'
    }
    options{
        timeout(time: 50, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }

    parameters{
        
        choice(name: 'ENVIRONMENT', choices: ['dev', 'qa', 'uat', 'pre-prod', 'prod'], description: 'Select your Environment')
        string(name: 'version',  description: 'Enter your application version')
        // string(name: 'jira-id',  description: 'Enter your jira id')
    }

    environment {
        
        appversion = ''
        region = 'us-east-1'
        acc_ID = '135808959960'
        project = 'expense'
        environment = ''
        component = 'backend'

    }
    
    stages {

        stage('Setup Environment'){
            steps{
                script{
                    environment = params.ENVIRONMENT
                    appversion = params.version
                    // account_id = pipelineGlobals.getAccountID(environment)
                }
            }
        }
        stage('Deploy Backend') {
            steps {
                withAWS(region: 'us-east-1', credentials: 'aws-cred') {
                    
                        sh """    
                            aws eks update-kubeconfig --region ${region} --name kdp-expense-prod-eks                    
                            cd helm
                            sed -i "s/IMAGEVERSION/${appversion}/g" values.yaml
                            helm upgrade --install backend-chart -n rnk-expense -f values.yaml .
                        """
                    
                }

            }
        }
        

    }
    








    post {
        always{
            echo 'this will run always'
            deleteDir()
        }
        success{
            echo 'this will run on success'
        }
        failure{
            echo 'this will run at failure'
        }
    }
}