pipeline {
    agent any
    stages {
        stage('Fetch') {
            steps {
                sh '''
                rm -rf vote_app
                git clone https://github.com/iam-Haresh/vote_app.git
                cd vote_app
                ls
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 597088025241.dkr.ecr.us-east-1.amazonaws.com
                docker build -t 597088025241.dkr.ecr.us-east-1.amazonaws.com/upgrad/assignment_vote:${VERSION} .
                docker push 597088025241.dkr.ecr.us-east-1.amazonaws.com/upgrad/assignment_vote:${VERSION}
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                IMAGE_NAME=597088025241.dkr.ecr.us-east-1.amazonaws.com/upgrad/assignment_vote:${VERSION}
                echo -e "services:\n  vote:\n    image: ${IMAGE_NAME}\n    ports:\n      - 80:80" > docker-compose.yaml
                docker-compose up
                '''
            }
        }
    }
}