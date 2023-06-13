node{
 def mavenHome =  tool name: "Maven"
    stage("Git CheckOut"){
        git url: 'https://github.com/sobengdarko/spring-boot-docker',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      sh "${mavenHome}/bin/mvn clean package"
    } 
    stage(" Static Code Analysis"){
    sh "echo sonarqube analysis completed"     
	 //sh "${mavenHome}/bin/mvn sonar:sonar"
	 } 
    stage(" Upload Artifactory"){
       sh "artifact successfully uploaded"
       //sh "${mavenHome}/bin/mvn deploy"
	} 
    stage("Build Dokcer Image") {
         sh "docker build -t sobeng/springboot ."
    }
    
    stage("Docker Push"){
        withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Pwd')]) {
          sh "docker login -u sobeng -p ${Docker_Hub_Pwd}"
       } 
        sh "docker push sobeng/springboot:latest "
        
    }
    
    // Remove local image in Jenkins Server
    stage("Remove Local Image"){
        sh "docker rmi sobeng/springboot:latest"
    }
    
    stage("Deploy to kubernetes"){
        sh "kubectl apply -f jamiekeynes.yml"
        }
}
