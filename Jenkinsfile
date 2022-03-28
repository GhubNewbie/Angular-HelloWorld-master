
pipeline {
    agent any
    tools {nodejs "node"}
    

    
    stages {
        try {
            notify('INITIALIZED')
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
        } catch (e) {
            currentBuild.result = "FAILED"
            throw e
        } finally {
            notify(currentBuild.result)
        }
        
    }
}

def notify(String buildStatus) {
            
    // send to email
    emailext (
        subject: "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
        body: """<p>${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
        recipientProviders: [[$class: 'DevelopersRecipientProvider']]
    )
}
