pipeline {
    
    agent { label "dev" };
        
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/sahmed448/two-tier-flask-app.git", branch: "master"
                echo "Git Repo is cloned to $pwd/workspace/demo-cicd"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t sahmed448/two-tier-flask-app:latest ."
            }
        }
        stage("Test"){
            steps{
                echo "Testing your app"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerHubCreds', usernameVariable: 'dockerHubUser', passwordVariable: 'dockerHubPass')]) 
                {
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app" 
            }
        }
    }
}
