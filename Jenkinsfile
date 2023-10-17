pipeline {
    agent { label 'docker-node-1' }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the GitHub repository's 'develop' branch
                checkout([$class: 'GitSCM', branches: [[name: '*/develop']], userRemoteConfigs: [[url: 'https://github.com/GitPracticeRepositorys/StudentCoursesRestAPI.git']]])
            }
        }

        stage('Kustomize') {
            steps {
                script {
                    def kustomizePath = tool name: 'kustomize', type: 'Tool Type Name' // Configure your Kustomize tool name

                    dir('path/to/kustomization/directory') {
                        // Use Kustomize to apply the configuration
                        sh "${kustomizePath}/kustomize build . | kubectl apply -f -"
                    }
                }
            }
        }
    }
}
