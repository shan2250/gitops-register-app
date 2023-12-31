pipeline {
    agent { label "Jenkins-agent" }
    environment {
        APP_NAME = "register-app-pipeline"
        IMAGE_TAG = "latest"  // Provide a default value or set it dynamically
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/shan2250/gitops-register-app.git'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                script {
                    sh """
                       cat deployment.yaml
                       sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                       cat deployment.yaml
                    """
                }
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                script {
                    sh """
                       git config --global user.name "shan2250"
                       git config --global user.email "sayyadshahrukh2250@gmail.com"
                       git add deployment.yaml
                       git commit -m "Updated Deployment Manifest"
                    """
                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                        sh "git push https://github.com/shan2250/gitops-register-app.git main"
                    }
                }
            }
        }
    }
}
