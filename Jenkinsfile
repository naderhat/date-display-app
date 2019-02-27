podTemplate(label: 'dojo-pod', containers: [
  containerTemplate(name: 'npm', image: 'node:carbon-jessie', command: 'cat', ttyEnabled: true)
])
 {
 node() {
    echo "Your Pipeline works!"
    stage('Build') {
      try {
        container('npm') {
          sh """
            pwd
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
