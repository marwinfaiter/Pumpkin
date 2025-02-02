pipeline {
    agent any
    stages {
        stage("Clean workspace") {
            steps {
                sh "git clean -xdf"
            }
        }
        stage("Build and push docker image") {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://releases.docker.buddaphest.se', 'nexus') {

                        def customImage = docker.build("marwinfaiter/pumpkin:${env.BUILD_ID}")

                        customImage.push()
                        customImage.push("latest")
                    }
                }
            }
        }
    }
    post {
        always {
            sh "docker system prune -af"
        }
    }
}
