def VERSION = "${env.BUILD_NUMBER}"
def DIST_ARCHIVE = "dist.${env.BUILD_NUMBER}"

pipeline {
    agent any
    tools {nodejs "node"}

    stages {
        stage('NPM Install') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm run build'
                sh 'npm run ng build'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['apache']){
                    sh "ssh ec2-user@18.207.200.20 rm -rf /var/www/html/dist/"
                    sh "ssh ec2-user@18.207.200.20 rmkdir -p /var/www/html"
                    sh "scp -r dist ec2-user@18.207.200.20:/var/www/html/dist/*"
                    sh "ssh ec2-user@18.207.200.20 “rm -rf /var/www/example.com/dist/ && mv /var/www/temp_deploy/dist/ /var/www/example.com/”"
                }
                
            }
        }
    }
}
