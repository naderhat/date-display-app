podTemplate(label: 'dojo-pod', containers: [
  containerTemplate(name: 'npm', image: 'node:carbon-jessie', command: 'cat', ttyEnabled: true)
])
 {
 node('dojo-pod') {
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
  }
}
