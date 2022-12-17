node{
   stage('SCM Checkout'){
     git 'https://github.com/sathiyaraj2004/my-app'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven2', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
      stage('Build Docker Imager'){
   sh 'docker build -t sathiyaraj2004/myweb:0.0.2 .'
   }
      stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u sathiyaraj2004 -p ${dockerPassword}"
    }
    sh 'docker push sathiyaraj2004/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   withCredentials([string(credentialsId: 'nexusPassword', variable: 'nexusPass')]) {
   sh "docker login -u admin -p ${nexusPass} 13.127.107.80:8083"
   sh "docker tag sathiyaraj2004/myweb:0.0.2 13.127.107.80:8083/damo:1.0.0"
   sh 'docker push 13.127.107.80:8083/damo:1.0.0'
   }
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   }
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest sathiyaraj2004/myweb:0.0.2' 
   }
}
