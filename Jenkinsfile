@Library("shared_lib") _

pipeline {
    
    agent { label "dev" };
        
    stages{
        stage("Code Clone"){
            steps{
                script{
                   clone("https://github.com/sahmed448/two-tier-flask-app.git", "master")
               }
            }
        }
        
        stage("Trivy File system Scan"){
            steps{
                sh "trivy fs . -o result.json"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app:latest ."
            }
        }
        
        stage("Test"){
            steps{
                echo "Testing your app"
            }
        }
        
        stage("Push to Docker Hub"){
            steps{
                script{
                    docker_push("dockerHubCreds","two-tier-flask-app")
                }  
            }
        }
        
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app" 
            }
        }
    }

    post{
        success{
            script{
                emailext from: 'javed7504@gmail.com',
                to: 'javed7504@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'javed7504@gmail.com',
                to: 'javed7504@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
    
}
