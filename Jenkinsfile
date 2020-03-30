pipeline {
    agent any
    stages {
         stage('Lint HTML') {
                steps {
                    sh 'tidy -q -e *.html'
                }
         }
         stage('Upload to AWS') {
              steps {
                retry(3) {
                    withAWS(region:'us-east-2',credentials:'aws-static') {
                        sh 'echo "Uploading content with AWS creds"'
                        s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'fernanalegria-static')
                    }
                }
              }
         }
         stage('Up and running') {
            steps {
                sleep(time: 10, unit: 'SECONDS') {
                    sh 'curl -X GET "http://fernanalegria-static.s3-website.us-east-2.amazonaws.com/"'
                }
            }
         }
    }
}