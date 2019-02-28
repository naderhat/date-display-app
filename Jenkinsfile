podTemplate(label: 'dojo-pod', containers: [
  containerTemplate(name: 'npm', image: 'node:carbon-jessie', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true)
],
volumes: [
  hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
])
 {
 node('dojo-pod') {
 	def myRepo = checkout scm
    def gitCommit = myRepo.GIT_COMMIT
    def gitBranch = myRepo.GIT_BRANCH
    echo "Your Pipeline works!"
    
    
    	stage('Clone') {
	    	try {
	    		container('npm') {
	    			sh """
	    				git clone https://github.com/naderhat/date-display-app.git
	    			   """
	    		}
	    	} catch (exc) {
	    		println "Failed to clone the repo"
	    		throw(exc)
	    	}
	    }
	    stage('Build') {
	      try {
	        container('npm') {
	          sh """
	            pwd
	            ls -la
	            cd date-display-app
	            npm install
	            npm test
	            """
	        }
	      }
	      catch (exc) {
	        println "Failed to test - ${currentBuild.fullDisplayName}"
	        throw(exc)
	      }
	    }
	    stage('Docker Hub') {
	    	container('docker') {
		        withCredentials([[$class: 'UsernamePasswordMultiBinding',
		          credentialsId: 'dockerhub',
		          usernameVariable: 'DOCKER_HUB_USER',
		          passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
			          sh """
			            docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}
			            docker build -t 'DOCKER_HUB_USER'/dojo-cicd-nader:${gitCommit} .
			            docker push 'DOCKER_HUB_USER'/dojo-cicd-nader:${gitCommit}
			          """
			      }
		     }
		}
	
  }
}
