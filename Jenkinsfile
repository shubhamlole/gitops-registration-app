pipeline{
    agent{'Jenkins-Agent'}
    environment{
        APP_NAME = "register-app-pipeline"
    }
    stages{
        stage{
            stage("Cleanup Workspace"){
                steps{
                    cleanWs()
                }
            }
        }
        stage("Checkout From SCM"){
            steps{
                git branch: 'main' , credentialsId: 'github' , url: 'https://github.com/shubhamlole/gitops-registration-app.git'
            }
        }
        stage("update the Deployment Tags"){
            steps{
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                   """
            }
        }
        stage("Push the changed deployment file to git"){
            steps{
                sh """
                git config --global user.name "shubhamlole"
                git config --global user.email "loleshubham46@gmail.com"
                git add deployment.yaml
                git commit -m "updated Deployment manifest"
                   """
                withCredentials([gitUsernamePassword(credentialsId: 'github' , gitToolName: 'Default')]){
                    sh "git push https://github.com/shubhamlole/gitops-register-app main"
                }
            }

        }
    }
}