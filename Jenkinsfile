pipeline{
    agent { label "dev"};
    stages{
        stage("code clone"){
            steps{
                git url: "https://github.com/mayankchhimwal/two-tier-flask-app", branch:"master"
            }
        }
        stage("Build"){
        steps{
            sh "docker build -t two-tier-flask-app ."
        }
        }
        stage("Test"){
            steps{
                echo "Developer/tester will test"
            }
        }
        stage("Push to Docker Hub"){
            steps{
               withCredentials([usernamePassword(credentialsId: "DockerHubCreds",passwordVariable: "dockerHubPass",usernameVariable: "dockerHubUser")])
                {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
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
    post{
        success{
            emailext(
                to: "mayankchhimwal1999@gmail.com, gagan.patwal.1@gmail.com, lalitmudila09@gmail.com",
                subject: "Build Successful",
                body: "Good News: Your build is successful"
                )
        }
        failure{
            emailext(
                to: "mayankchhimwal1999@gmail.com, gagan.patwal.1@gmail.com, lalitmudila09@gmail.com", 
                subject: "Build Failed",
                body: "Bad News: Your build is Failed"
                )
        }
    }
}
