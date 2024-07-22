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
                sh 'git checkout -b main origin/main'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                  cd k8s/$SERVICE_NAME
                  ls
                  sed -i "s|image: .*|image: ${IMAGE_FULL_NAME_PARAM}|" frontend.yaml
                  git checkout -b main origin/main
                  git add frontend.yaml
                  git commit -m "changed image version"
                  git push origin main
                '''
            }
        }
    }
}