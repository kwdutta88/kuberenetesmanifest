pipeline {
    agent any

    stages {
        stage('Clone repository') {
            steps {
                script {
                    // Use 'withCredentials' to access GitHub credentials for cloning
                    withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        checkout scm
                    }
                }
            }
        }

        stage('Update GIT') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        // Set Git configuration
                        sh "git config user.email krishnakali.dutta8817@gmail.com"
                        sh "git config user.name kwdutta88"
                        
                        // Display content of deployment.yaml before modification
                        sh "cat deployment.yaml"
                        
                        // Update deployment.yaml with the new Docker image tag
                        sh "sed -i 's+krishdutta1177/test.*+krishdutta1177/test:${DOCKERTAG}+g' deployment.yaml"
                        
                        // Display content of deployment.yaml after modification
                        sh "cat deployment.yaml"
                        
                        // Git operations using injected credentials
                        withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                            sh "git add ."
                            sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
                        }
                    }
                }
            }
        }
    }
}
