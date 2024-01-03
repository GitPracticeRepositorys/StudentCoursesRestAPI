pipeline {
    agent { label 'docker-node-1' }
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
        stage('Kustomize Deploy') {
            agent { label 'docker-node' }
            steps {
                // Use Kustomize to apply the Kubernetes configuration
                sh "cd deployments"
                sh 'kubectl apply -f deployments'
            }
        }
    }
}
