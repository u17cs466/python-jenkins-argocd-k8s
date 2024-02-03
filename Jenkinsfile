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
                    docker build -t srikanth2233/damacharla44:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Docker Build and Push') {
            steps {
                script {
                    sh '''
                    echo Srikanth@#123 | docker login -u srikanth.damaharla99@gmail.com --password-stdin docker.io
                    docker push srikanth2233/damacharla44:${BUILD_NUMBER}



                    '''
                    // // Log in to Docker Hub
                    // docker.withRegistry('https://index.docker.io/v1/', docker-cred ) {
                    //     // Pull the Docker image from the private repository (if needed)
                    //     docker.image('srikanth2233/damacharla44').pull()

                    //     // Your Docker-related steps go here
                    //     // For example, docker build and push commands
                    //     docker.build('srikanth2233/damacharla44').push()
                    // }
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
