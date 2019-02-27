podTemplate(label: 'dojo-pod', containers: [
  containerTemplate(name: 'npm', image: 'node:carbon-jessie', command: 'cat', ttyEnabled: true)
])
 {
 node('dojo-pod') {
    echo "Your Pipeline works!"
    environment {
        DOCKER_USERNAME = credentials('jenkins-aws-secret-key-id')
        DOCKER_PASSWORD = credentials('jenkins-aws-secret-access-key')
    }
    stages {
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
		            docker build -t namespace/my-image:${gitCommit} .
		            docker push namespace/my-image:${gitCommit}
		          """
		        }
		      }
		    }
	    }
	}
  }
}
