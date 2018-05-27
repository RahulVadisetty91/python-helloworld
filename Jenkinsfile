pipeline {
    
 agent any
    stages{
        
        stage ('Tests') {
		steps {
			parallel ( "unit tests": { sh 'echo  "mvn test"' },
                    "integration tests": { sh 'echo "mvn integration-test"' }
                )
        }
	}
      	stage ('Deploy') {
		steps {
            		sh "python code_holder/helloworld.py"
		}
      	}
	stage ('artifact') {
		steps {
            		sh "tar -zcvf code_holder.tar.gz code_holder"
		}
      	}  
     } 
  post { 
        success { 
            echo 'Tring to upload artifactory'
            azureUpload storageCredentialId: 'azurestorageaccount', storageType: 'blobstorage', containerName: 'pythondatabricks', filesPath: 'code_holder.tar.gz', virtualPath: '$BUILD_ID/$BUILD_NUMBER/code_holder.tar.gz'
        }
		
       }   
 }
