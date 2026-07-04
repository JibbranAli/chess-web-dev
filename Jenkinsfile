pipeline {
    agent any
    
    stages {
        stage('Checkout Source Code') {
            steps {
                // Fixed typo: 'chekout' -> 'checkout'
                checkout scm
            }
        }
        
        stage('Deploy to testing server') {
            steps {
                sh '''
                echo "Installing Apache"
                sudo yum install httpd -y
                sudo mkdir -p /var/www/html 
                sudo cp index.html /var/www/html/
                sudo systemctl stop firewalld
                '''
            }
        }
        
        stage('Technical Team Approval') {
            steps {
                // Added missing comma between parameters
                input(
                    message: 'Technical Team is Testing Successfully ?',
                    ok: 'Approve Deployment'        
                )
            }
        }
        
        stage("Deploy to Production") {
            // Removed duplicate 'steps' keyword
            steps {
                sh '''
                echo "Installing Apache"
                sudo yum install httpd -y
                sudo mkdir -p /var/www/html 
                sudo cp index.html /var/www/html/
                sudo systemctl start httpd 
                sudo systemctl stop firewalld
                '''
            }
        }
        
        stage('Manager Approval') {
            steps {
                // Added missing comma between parameters
                input(
                    message: 'Manager Approve Production Release ?',
                    ok: 'Release'
                )
            }
        }
        
        stage('Production release complete') {
            steps {
                echo "Application Successfully Deployed BY Jenkins"
            }
        }
    } // Added missing closing brace for 'stages' block
    
    post {    
        success {
            // Changed 'emailtext' to standard 'emailext' step
            emailext(
                to: 'intern.linuxworld@gmail.com',
                subject: 'Production Deployment Successfully Done',
                body: '''
                Hello Team 
                
                Production Deployment done 
                
                All stages worked 
                '''
            )
        }
        failure {
            // Changed 'emailtext' to standard 'emailext' step
            emailext(
                to: 'intern.linuxworld@gmail.com',
                subject: 'Production Deployment failed',
                body: '''
                Hello Team 
                
                Production Deployment failed 
                
                All stages NOT Worked    
                '''    
            )
        }
    }
}