pipeline {
    agent {
        label 'build'
    }
        post {
        always {
            cleanWs()
        }
    }
    environment {
        REPO_URL = 'git@github.com:Mikola1911/lasertower.git'
        BRANCH = 'master'
        CLONE_DIR = '/'
        NEXUS_URL = 'http://1177733-ca24368.tw1.ru/:8123'
        NEXUS_USERNAME = credentials('nexus-username')
        NEXUS_PASSWORD = credentials('nexus-password')
    }
    stages {
        stage('Build and Push') {
            steps {
                sh 'docker stop $(docker ps -aq)'
                sh 'docker system prune -a --volumes'
                dir(CLONE_DIR) {
                    git branch: BRANCH, url: REPO_URL
                    dir('env/dev') {
                        sh 'docker-compose up -d --build'
                    }
                }
                script {
                    // Push images to Nexus
                    withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                        sh "docker login -u ${NEXUS_USERNAME} -p ${NEXUS_PASSWORD} ${NEXUS_URL}"
                        sh "docker-compose push"
                        sh "docker logout ${NEXUS_URL}"
                    }
                }
            }
        }
stage('Deploy on Master Node') {
    agent {
        label 'master'
    }
    environment {
        MASTER_CLONE_DIR = '/'
    }
    steps {
        dir(MASTER_CLONE_DIR) {
            git branch: BRANCH, url: REPO_URL
            dir('env/prod') {
                sh 'docker-compose up -d'
                sh 'docker-compose ps -a'
                    }
                }
            }
        }
    }
}
