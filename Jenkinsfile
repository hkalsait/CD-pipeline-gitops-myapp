pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "gitops-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                  
                   git branch: 'master', credentialsId: 'github', url: 'https://github.com/hkalsait/CD-pipeline-gitops-myapp'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "harshavardhan"
                   git config --global user.email "harshavardhan@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                   sh "git push --set-upstream https://github.com/hkalsait/CD-pipeline-gitops-myapp.git master"
                }
            }
        }
      
    }
}
