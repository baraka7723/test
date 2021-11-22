pipeline{
    agent any
    
    //environment{
       // dockerImage = ''
       // registry = 'baraka7723/test'
       
   // }
    
    
    stages{
        stage('checkpoint'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/baraka7723/test']]])
            }
        }
        
        stage('build docker image'){
            steps{
                sh 'docker build -t baraka7723/test:1.0.0 .'
            }
        }
        
        stage('Upload Image'){
            steps{
                withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]) {
                sh "docker login -u baraka7723 -p ${dockerHubPwd}"
                }
                sh 'docker push baraka7723/test:1.0.0'
            }
            
        }
        stage('Stopping Docker containers for cleaner Docker run') {
              steps{
               sh 'docker ps -f name=my-app -q | xargs --no-run-if-empty docker container stop'
               sh 'docker container ls -a -fname=my-app -q | xargs -r docker container rm' 
            }  
            
            
        }
        stage ('Deploy'){
            steps{
                sh 'docker run -d  --rm -p 8081:5000 --name my-app baraka7723/test:1.0.0'
            }
        }
    }
}
