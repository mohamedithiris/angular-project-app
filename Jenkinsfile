pipeline {
    agent any

    environment {
        AWS_CLI = "${env.AWS_CLI}"
    }

    tools {
        nodejs 'nodejs' // Assuming 'nodejs' matches the name of the Node.js installation defined in Jenkins Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from Git
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/mohamedithiris/angular-project-app']]
                )
            }
        }

        stage('npm install') {
            steps {
                // Install dependencies
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                // Specify the full path to ng executable
                sh 'ng build --configuration=production'
                echo 'Angular build successful!'
            }
        }

        stage('Upload to S3') {
            steps {
                // Use the AWS CLI path to execute AWS commands
                sh "${env.AWS_CLI}/aws s3 sync dist/angular-project s3://angular-project-app"
            }
            post {
                success {
                    // Echo the static website URL
                    echo 'Static website URL: http://angular-project-app.s3-website-us-east-1.amazonaws.com'
                }
            }
        }
    }
}
