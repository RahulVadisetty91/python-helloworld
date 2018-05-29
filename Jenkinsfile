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
            echo 'Tring to upload artifact'
	    withCredentials([azureServicePrincipal('Azure_Prod_SPN')]) {
            sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID'
            sh 'az account set -s $AZURE_SUBSCRIPTION_ID'
            sh 'az account show'
            sh 'az storage blob list --container-name pythondatabricks --output table'
 	    sh  'az storage blob upload --account-name pythondatabricks --container-name pythondatabricks --type page --file code_holder.tar.gz --name code_holder.tar.gz'
        }
            //azureUpload storageCredentialId: 'azurestorageaccount', storageType: 'blobstorage', containerName: 'pythondatabricks', filesPath: 'code_holder.tar.gz', virtualPath: '${BUILD_ID}/${BUILD_NUMBER}'
        }
		
       }   
 }
