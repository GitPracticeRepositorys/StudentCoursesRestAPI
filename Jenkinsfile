pipeline {
    agent { label 'docker-node-1' }
    triggers { pollSCM('* * * * *') }
    stages {
        stage('vcs') {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: 'develop']], userRemoteConfigs: [[url: 'https://github.com/GitPracticeRepositorys/StudentCoursesRestAPI.git']])
                }
            }
        }
        stage('build and push') {
            steps {
                script {
                    sh "docker image build -t shivakrishna99/courses:demo_${BUILD_ID} ."
                    sh "docker image push shivakrishna99/courses:demo_${BUILD_ID}"
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    dir("${WORKSPACE}/StudentCoursesRestAPIopsk8s") {
                        sh "yq eval -i '.spec.template.spec.containers[0].image = \"shivakrishna99/courses:demo_${BUILD_ID}\"' deployment.yaml"
                        sh """
                            git add deployment.yaml
                            git commit -m "Update container image"
                            git push origin main
                        """
                    }
                }
            }
        }
    }
}
