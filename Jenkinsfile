def VERSION = "${env.BUILD_NUMBER}"
def DIST_ARCHIVE = "dist.${env.BUILD_NUMBER}"

pipeline {
    agent any
    tools {nodejs "node"}

    stages {
        stage('NPM Install') {
            steps {
                sh 'npm install -g'
                sh 'npm install -g @angular/cli'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm audit fix --force'
                sh 'npm run build'
                sh 'nmp run ng build'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['ansible_demo']){
                    sh "ssh ec2-user@52.90.245.71 rm -rf /var/www/html/dist/"
                    sh "ssh ec2-user@52.90.245.71 rmkdir -p /var/www/html"
                    sh "scp -r dist ec2-user@52.90.245.71:/var/www/html/dist/*"
                    sh "ssh user@server “rm -rf /var/www/example.com/dist/ && mv /var/www/temp_deploy/dist/ /var/www/example.com/”"
                }
                
            }
        }
    }
}
