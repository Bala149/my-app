node{
   stage('Greens'){
     git 'https://github.com/Bala149/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'bala3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'bala3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Imager'){
   sh 'docker build -t bala149/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u bala149 -p ${dockerPassword}"
    }
   sh 'docker push bala149/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 15.207.222.64:8083"
   sh "docker tag bala149/myweb:0.0.2 15.207.222.64:8083/damo:1.0.0"
   sh 'docker push 15.207.222.64:8083/damo:1.0.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f ganimage'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8070:8080 --name ganimage bala149/myweb:0.0.2' 
   }
}
}
