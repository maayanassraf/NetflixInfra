pipeline {
    agent {
       label 'general'
    }

    parameters {
        string(name: 'SERVICE_NAME', defaultValue: 'NetflixFrontend', description: '')
        string(name: 'IMAGE_FULL_NAME_PARAM', defaultValue: '', description: '')
    }

    stages {
        stage('Git Setup') {
            steps {
                sh 'git checkout main'
                sh 'git pull'
            }
        }
        stage('Update Yaml') {
            steps {
                sh '''
                  cd k8s/$SERVICE_NAME
                  ls
                  sed -i "s|image: .*|image: ${IMAGE_FULL_NAME_PARAM}|" frontend.yaml
                  git add frontend.yaml
                  git commit -m "changed image version"
                '''
            }
        }
        stage('Git Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GITHUB_USERNAME', passwordVariable: 'GITHUB_TOKEN')]) {
                    sh 'git checkout -b main origin/main'
                }
            }
        }
    }
}