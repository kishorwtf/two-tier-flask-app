@Library ("Shared") _
pipeline{
    
    agent any;
    
    stages{
        stage("Code Clone"){
            steps{
                script {
                    clone("https://github.com/kishorwtf/two-tier-flask-app.git", "master")
                }
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
                echo "Docker Build Bhi Ho Gaya.."
            }
        }
        stage("trivy file system scan"){
            steps{
                script{
                    trivy_fs()
                }
            }
        }
        stage("Test"){
            steps{
                echo "Developer/Tester tests likh ke dega.."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                script {
                    docker_push("dockerHubCreds","two-tier-flask-app")
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
                echo "Docker Compose se Deploy bhi hogaya.."
            }
        }
    }
    
post {
    success {
        emailext body: 'Good News! : The build was successful!',
                 subject: 'Build Successful',
                 to: 'kishorwtf@gmail.com'
    }
    failure {
        emailext body: 'Bad News! : The build has failed',
                 subject: 'Build Failed',
                 to: 'kishorwtf@gmail.com'
    }
}
}
