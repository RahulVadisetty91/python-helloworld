pipeline {
    
 
    try {
        
        stage ('Tests') {
	        parallel 'static': {
	            sh "echo 'shell scripts to run static tests...'"
	        },
	        'unit': {
	            sh "echo 'shell scripts to run unit tests...'"
	        },
	        'integration': {
	            sh "echo 'shell scripts to run integration tests...'"
	        }
        }
      	stage ('Deploy') {
            sh "python helloworld.py"
			
      	}
    } catch (err) {
        currentBuild.result = 'FAILED'
        throw err
    }
   
 }
