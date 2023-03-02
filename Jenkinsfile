pipeline {
    agent any
    // agent {label "slave1"}
    stages {
        stage('CHECKOUT SCM') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: 'main']],
                    userRemoteConfigs: [[
                        url: 'https://git@github.com:MarkeyBass/JenkinsMaster.git',
                        credentialsId: ''
                    ]]
                ])
            }
        }

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
        stage('PreUploadToGit') {
            steps {
                sh """
                    gzip -d dynamoDB.json.gz
                    git config --global user.name "MarkeyBass"
                    git config --global user.email "markeybass@gmail.com"
                    git clone git@github.com:MarkeyBass/JenkinsMaster.git
                    cp dynamoDB.json JenkinsMaster
                    cd JenkinsMaster
                    ls

                    echo "\nhello" >> README.md
                    git status
                    git add .
                    git commit -m "some message"
                    git status
                    git status
                """
            }

        }
        stage('GitPush') {
            steps {
                sh """
                    cd JenkinsMaster
                    git push origin main
                    git status
                """
            }
        }
        stage('Python'){
            steps{
                sh "echo THIS IS PYTHON VERSION"
                sh "python3 --version"
            }
        }
    }
    post {
        always {
            deleteDir()
        }
    }

}


