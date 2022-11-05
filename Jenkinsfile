node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/MithunTechnologiesDevOps/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t dockerhandson/java-web-app .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'docker_hub_password', variable: 'dockerpassword')]) {
          sh "docker login -u dockerhandson -p ${Docker_Hub_Pwd}"
        }
        sh 'docker push dockerhandson/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app dockerhandson/java-web-app'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@15.206.81.146 docker stop java-web-app || true'
          sh 'ssh  ubuntu@15.206.81.146 docker rm java-web-app || true'
          sh 'ssh  ubuntu@15.206.81.146 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@15.206.81.146 ${dockerRun}"
       }
       
    }
     
     
}
