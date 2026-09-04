pipeline{
    
    agent any;
    
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/kishorwtf/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
                echo "Docker Build Bhi Ho Gaya.."
            }
        }
        stage("Test"){
            steps{
                echo "Developer/Tester tests likh ke dega.."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app"
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
}
