pipeline {

    agent any
    
    stages{

        stage('Build'){
            steps{
                echo 'Building...'             
                git credentialsId: 'git_credentials', url: 'https://github.com/bhajduk/delta-chat'
                dir('Docker'){
                    
                    sh '''
                        curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o ~/docker-compose
                        chmod +x ~/docker-compose
                        ~/docker-compose up -d build-agent
                    ''' 
                }
            }
            
            post {
                    success {
                        emailext attachLog: true, 
                            body: "Build status: ${currentBuild.currentResult}, Job: ${env.JOB_NAME}", 
                            recipientProviders: [developers()], 
                            subject: 'Build stage passed', 
                            to: 'unival12@gmail.com'
                    }

                    failure {
                        emailext attachLog: true, 
                            body: "Build status: ${currentBuild.currentResult}, Job: ${env.JOB_NAME}", 
                            recipientProviders: [developers()], 
                            subject: 'Build stage failed', 
                            to: 'unival12@gmail.com'
                    }
                }
        }
        
        stage('Test') {      
            steps{         
                echo 'Testing...'
                dir('Docker'){
                    sh '~/docker-compose up -d test-agent'
                }
            }
            
            post {
                success {
                    emailext attachLog: true, 
                        body: "Test status: ${currentBuild.currentResult}, Job ${env.JOB_NAME}", 
                        recipientProviders: [developers()], 
                        subject: 'Test stage passed', 
                        to: 'unival12@gmail.com'
                }

                failure {
                    emailext attachLog: true, 
                        body: "Test status: ${currentBuild.currentResult}, Job ${env.JOB_NAME}", 
                        recipientProviders: [developers()], 
                        subject: 'Test stage failed', 
                        to: 'unival12@gmail.com'
                }
            }
        }
    }

    post {
        success {
            emailext attachLog: true, 
                body: "Pipeline status: ${currentBuild.currentResult}, Job ${env.JOB_NAME}", 
                recipientProviders: [developers()], 
                subject: 'Whole pipeline passed', 
                to: 'unival12@gmail.com'
        }

        failure {
            emailext attachLog: true, 
                body: "Pipeline status: ${currentBuild.currentResult}, Job ${env.JOB_NAME}", 
                recipientProviders: [developers()], 
                subject: 'Whole pipeline failed', 
                to: 'unival12@gmail.com'
        }
    }
}
