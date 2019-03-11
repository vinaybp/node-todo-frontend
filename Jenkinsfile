pipeline {
  environment{
    registry = "vinay4790/testrepo"
    registryCredential = 'dockerhub'
    dockerImage = ''
    containerId = sh(script: 'docker ps -aqf "name=node-app"', returnStdout:true)	
	 
   
  }
  agent any
  tools {nodejs "node"}
    
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/vinaybp/node-todo-frontend'
      }
    }
        
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }
     
    stage('Test') {
      steps {
         sh 'npm test'
      }
    }
    stage('Building Image'){
      steps{
        script{
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    
    stage('Push Image'){
      steps{
        script{
          docker.withRegistry('',registryCredential) {
            dockerImage.push()
          }
        }
      }
    }
    
    stage('Cleanup'){
      when{
        not {environment ignoreCase:true, name:'containerId', value:''}
      }
      steps {
        sh 'docker stop ${containerId}'
        sh 'docker rm ${containerId}'
      }
    }
    stage('Run Container'){
      steps{
        sh 'docker run --name=node-app -d -p 3000:3000 $registry:$BUILD_NUMBER &'
      }
    }
    
   /* stage('Upload art'){
	    def server = 'jfrog'
		def rtDocker = Artifactory.docker server: server
		def buildInfo = rtDocker.push ('http://52.172.31.12:8081/artifactory/dockerImage', 'example-repo-local')
		server.publishBuildInfo buildInfo
    }*/
	  
	  /*stage('Deploy the application'){
	  steps{
	  
		  //Deploying the docker image as a service using kubernetes CD plugin
		  //Method to deploy the tyaml file
		  kubernetesDeploy(
			  kubeconfigId: 'kubeconfig',
			  configs: 'Application.yml',
			  enableConfigSubstitution: false
			  )
	  }
	  }*/
	  
    
  }
}
