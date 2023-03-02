pipeline {
    agent any
    // agent {label "slave1"}
    stages {

        stage('S3download') {
            steps {
                sh 'rm -rf ./*'
                withAWS(credentials: 'awscredentials', region: '') {
                    s3Download(
                        file: 'dynamoDB.json.gz',
                        bucket: 'lesson10-bucket',
                        path: "AWSDynamoDB/01676569442425-60a32f40/data/cdpeid643q2ezh4hcgcza2afx4.json.gz"
                    )
                }
            }
        }

        stage('Python'){
            steps{
                sh "echo THIS IS PYTHON VERSION"
                sh "python3 --version"
                sh "python3 first_example.py"
            }
        }
    }
    post {
        always {
            deleteDir()
        }
    }

}


