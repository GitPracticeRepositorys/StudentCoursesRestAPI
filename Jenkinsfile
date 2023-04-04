pipeline {
    agent { label 'docker' }
    triggers { pollSCM('* * * * *') }
    stages {
        stage('vcs') {
            steps {
                git branch: 'develop',
                    url: 'https://github.com/GitPracticeRepositorys/StudentCoursesRestAPI.git'
            }
        }
        stage('build and deploy') {
            steps {
                sh "docker image build -t shivakrishna99/courses:develop-$env.BUILD_ID ."
                sh "docker image push shivakrishna99/courses:develop-$env.BUILD_ID"
            }
        }
        stage('deploy') {
            steps {
                sh "cd deployments/courses/overlays/develop && kustomize edit set image courses=shivakrishna99/courses:develop-$env.BUILD_ID"
                sh 'kubectl apply -k deployments/courses/overlays/develop'
            }
        }

    }
}
