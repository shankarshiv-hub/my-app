-node{
   stage('SCM Checkout'){
     git 'https://github.com/shankarshiv-hub/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }

   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }

   stage('Build Docker Imager'){
   sh 'docker build -t 11161111/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u 11161111 -p ${dockerPassword}"
    }
   sh 'docker push 11161111/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 3.128.181.242:8083"
   sh "docker tag 11161111/myweb:0.0.2 3.128.181.242:8083/mulsan:1.0.0"
   sh 'docker push 3.128.181.242:8083/mulsan:1.0.0'
   }
