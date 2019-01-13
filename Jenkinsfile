#!groovy
node ('worker_node1') {
   // always run with a new workspace
   step([$class: 'WsCleanup'])
   try {
      stage('Source') {
	 checkout scm
	 stash name: 'test-sources', includes: 'build.gradle,src/test/'
      }
      stage('Build') {
         // Run the gradle build
         sh 'gradle clean build'
      }
      stage ('Test') {
      // execute required unit tests in parallel
      
         parallel (
            worker1: { node ('master'){
               // always run with a new workspace
               step([$class: 'WsCleanup'])
	       unstash 'test-sources'		
	       sh 'gradle -D test.single=TestExample1 test'
            }},
            worker2: { node ('worker_node1'){
               // always run with a new workspace
               step([$class: 'WsCleanup'])
               unstash 'test-sources'
  	       sh 'gradle -D test.single=TestExample2 test'
            }},
         ) 
      } 
   } 
   catch (err) {
      echo "Caught: ${err}"
   }
   stage ('Notify') {
     // mailUser('<your email address>', "Finished")
   }
   
}

