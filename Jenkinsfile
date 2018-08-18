#!groovy
node ('worker_node1') {
   // always run with a new workspace
   step([$class: 'WsCleanup'])
   try {
      stage('Source') {
	 checkout scm
      }
      stage('Build') {
         // Run the gradle build
         sh 'gradle clean build'
      }      
   } 
   catch (err) {
      echo "Caught: ${err}"
   }
   stage ('Notify') {
     // mailUser('<your email address>', "Finished")
   }
   
}

