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
                    sh 'ssh -o StrictHostKeyChecking=no ec2-user@18.207.200.20 "rm -rf /var/www/html/dist/"'
                    sh 'ssh -o StrictHostKeyChecking=no ec2-user@18.207.200.20 "mkdir -p /var/www/html"'
                    sh 'scp -r ${WORKSPACE}/dist/* ec2-user@18.207.200.20:/var/www/html/'
                }
                
            }
        }
    }
}
