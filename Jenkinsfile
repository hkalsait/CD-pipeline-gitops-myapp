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
                   set +e
                   git config --global user.name "hkalsait"
                   git config --global user.email "harsh5kalsait@gmail.com"
                   git init
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                   git push https://github.com/hkalsait/CD-pipeline-gitops-myapp master"
                """
               withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                   sh "git push https://github.com/hkalsait/CD-pipeline-gitops-myapp master"
                }
            }
        }
      
    }
}
