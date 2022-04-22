pipeline {
    agent any
    environment {
		//PATH = "${PATH}:${getMavenPath()}"
		DOCKER_TAG = "${getLatestCommitId()}"
		//NEXUS_HOST = "172.31.45.145:8083"
		DEV_IP = "65.0.124.2"
	}
    stages {
	
	//stage("Git Checkout"){
            //steps{
                //git  credentialsId: 'githubcreds', url: 'https://github.com/lihkas/devops-JDG.git'
            //}
        //} 
	
        stage('DOCKER-BUILD') {
             steps {
                                  script{
                                         //sh 'cp -r ../shared-library@2/target .'
                                           //sh "docker build . -t ${NEXUS_HOST}/myweb:${DOCKER_TAG} "
			                                //}
                                           //sh 'docker build . -t neekohslihka/akhil-testone:alpha2'
                                             sh 'docker build . -t neekohslihka/akhil-testone:${DOCKER_TAG}' 
                                           //withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhubcreds')]) {

                                              //sh 'docker login -u neekohslihka -p $dockerhubcreds'
                                              //sh 'docker push neekohslihka/akhil-testone:${DOCKER_TAG}'
                                          	//}
                                  	}
        		}
    		}
	 stage('DOCKER_PUSH'){
			steps{
				script {
					withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhubcreds')]) {

                                              sh 'docker login -u neekohslihka -p $dockerhubcreds'
                                              sh 'docker push neekohslihka/akhil-testone:${DOCKER_TAG}'
                                          	}
				}
			}
		}
		
		stage('Docker - Deploy - Dev'){
			steps{
			    // input 'Do you want to deploy to dev servers?'
				script {
				sh returnStatus: true, script: "ssh akhil@${DEV_IP} docker rm -f dockercontainer"
				withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhubcreds')]) {

                                              //sh 'docker login -u neekohslihka -p $dockerhubcreds'
					      sh "ssh akhil@${DEV_IP} docker run -d -p 8080:8080 --name=dockercontainer neekohslihka/akhil-testone:${DOCKER_TAG}"	
				}
				//withCredentials([string(credentialsId: 'nexus-docker', variable: 'nexusPwd')]) {
					//sh returnStatus: true, script: "ssh ec2-user@${DEV_IP} docker login -u admin -p ${nexusPwd} ${NEXUS_HOST}"
			    //}
				
				//sh returnStatus: true, script: 'ssh ec2-user@${DEV_IP} docker rmi $(docker images | grep 172.31.45.145:8083/myweb | awk \'{print $3}\')'
				//sh "ssh ec2-user@${DEV_IP} docker run -d -p 8080:8080 --name=myweb ${NEXUS_HOST}/myweb:${DOCKER_TAG}"	
					}
				}		
		}   
}
}
//to get docker image tag as per git commit id
def getLatestCommitId(){
	def commitId = sh returnStdout: true, script: 'git rev-parse HEAD'
	//def commitId = sh script: 'git rev-parse HEAD'
	return commitId
}
//to add maven home path
//def getMavenPath(){
//	def mvnHome = tool name: 'maven-3', type: 'maven'
//	return "${mvnHome}/bin"
//}
