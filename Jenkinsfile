pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        
    }
    
    stages {
        
        stage('Checkout'){
           steps {
               sh '''
               echo passed
               git --version
               echo pwd
               echo pass
               '''
                // git credentialsId: 'newtoken', 
                // url: 'https://github.com/u17cs466/python-jenkins-argocd-k8s.git',
                // branch: 'main'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                  def customImage = docker build -t srikanth2233/damacharla44:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Docker Build and Push') {
            steps {
                script {
                    
                    // Log in to Docker Hub
                   docker.withRegistry("https://registry.hub.docker.com", 'docker-cred') {
                        customImage.push()
                    }
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: 'newtoken', 
                url: 'https://github.com/u17cs466/python-jenkins-argocd-k8s.git',
                branch: 'main'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    withCredentials([usernamePassword(credentialsId: 'newtoken', passwordVariable: 'Srikanth@#123', usernameVariable: 'srikanth.damacharla99@gmail.com')]) {
                        sh '''
                        cat deploy.yaml
                        sed -i '' "s/32/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/u17cs466/python-jenkins-argocd-k8s.git HEAD:main
                        '''                        
                    }
                }
            }
        }
    }
}
