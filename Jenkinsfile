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
               echo pass
               '''
                // git credentialsId: '9ef05e1b-3b5f-4fec-9fcc-ec9233fecd4e', 
                // url: 'https://github.com/u17cs466/python-jenkins-argocd-k8s.git',
                // branch: 'main'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t srikanth1122/damacharla44:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                     docker.withRegistry('https://index.docker.io/v1/',$dockertoken) {
                        // For example, docker build and push commands
                        docker.build('srikanth1122/damacharla44:${BUILD_NUMBER').push()
                    }
                    // docker push srikanth2233/damacharla44:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: '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', 
                url: 'https://github.com/u17cs466/python-jenkins-argocd-k8s.git',
                branch: 'main'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    withCredentials([usernamePassword(credentialsId: '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', passwordVariable: 'Srikanth@#123', usernameVariable: 'u17cs466')]) {
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
