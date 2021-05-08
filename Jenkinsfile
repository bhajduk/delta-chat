pipeline {
    agent any  
    stages{

        stage('Build'){
            steps{
                echo 'Building...'
                git branch: 'Grupa02-BH306443_Lab07', url: 'https://github.com/InzynieriaOprogramowaniaAGH/MIFT2021'
                dir('Grupy/Grupa02/BH306443/Lab07/Docker'){
                    
                    sh '''
                        curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o ~/docker-compose
                        chmod +x ~/docker-compose
                        ~/docker-compose up -d build-agent
                    ''' 
                }
            }
        }
        
        stage('Test') {   
            steps{
                echo 'Testing...'
                dir('Grupy/Grupa02/BH306443/Lab07/Docker'){
                    sh '~/docker-compose up -d test-agent'
                }
            }
        }
    }
    
    post {
        success {
        	echo 'success'
            emailext attachLog: true, 
                body: "Status: ${currentBuild.currentResult}: Job ${env.JOB_NAME}", 
                recipientProviders: [developers()], 
                subject: 'Test passed', 
                to: 'unival12@gmail.com'
        }

        failure {
        	echo 'failure'
            emailext attachLog: true, 
                body: "Status: ${currentBuild.currentResult}: Job ${env.JOB_NAME}", 
                recipientProviders: [developers()], 
                subject: 'Test failed', 
                to: 'unival12@gmail.com'
        }
    }
}
