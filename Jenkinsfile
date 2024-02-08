pipeline
{
    agent any
    tools
    {
        maven 'Maven-3.9.6-ver'
    }
    stages
    {
        
        stage('Code checkout')
        {
            steps
            {
                sh 'echo "Hello Jenkins"'
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ashwin2041/springboot-k8s-example.git']])
                sh 'mvn clean install'
                
            }
        }
        
        stage('build docker image')
        {
            steps
            {
                sh 'docker build -t ashwin2041/springboot-k8s-example . '
            }
        }
        
        stage('push image to dockerhub')
        {
            steps
            {
                
                script
                {
                    withCredentials([string(credentialsId: 'github-token', variable: 'dockerhubsecret')]) {
                    // some block
                    sh 'docker login -u ashwin2041 -p ${dockerhubsecret}'
                    
                    }
                    sh 'docker push ashwin2041/springboot-k8s-example'
                }
            }
        }
        stage('deploy to minikube')
        {
              steps
              {
                   script
                  {
                         sh 'whoami'
                        sh 'pwd'
                      sh 'echo "======================="'
                   kubeconfig(credentialsId: 'kubernetes-secret-file', serverUrl: 'https://192.168.49.2:8443') {
                        sh 'whoami'
                        sh 'pwd'
                    // some block
                    sh 'kubectl apply -f deployment-service.yml'
                        
}
                  }
                  
              }   
        }  
    }
}
