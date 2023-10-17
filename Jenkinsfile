pipeline {
    agent any
    triggers { pollSCM('* * * * *') }
    stages {
        stage('vcs') {
            steps {
                script {
                    git branch: 'develop',
                        url: 'https://github.com/GitPracticeRepositorys/StudentCoursesRestAPI.git'
                }
            }
        }
        stage('build and deploy') {
            steps {
                script {
                    sh "docker image build -t shivakrishna99/courses:demo_${BUILD_ID} ."
                    sh "docker image push shivakrishna99/courses:demo_${BUILD_ID}"
                }
            }
        }
        stage('Deploy to kubernetes') {
            steps {
                script {
                    sh "cd ~/StudentCoursesRestAPIopsk8s && yq eval -i '.spec.template.spec.containers[0].image = \"shivakrishna99/courses:develop_${BUILD_ID}\" ' ~/StudentCoursesRestAPIopsk8s/deployment.yaml"
                    sh """
                      cd ~/StudentCoursesRestAPIopsk8s
                      git add deployment.yaml
                      git commit -m "added new change"
                      git push origin main
                      sleep 30
                    """
                }
            }
        }
    }
}
