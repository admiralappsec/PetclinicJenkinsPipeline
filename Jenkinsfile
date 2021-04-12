pipeline {
	agent any
	//this pipeline job assumes you have java jdk8 and maven3 configured
    	tools {
        	jdk 'jdk8'
        	maven 'maven3'
    	}

    	stages {
		stage ('Clone the project') {
    	    		steps{
				//this step clones an existing github project into your working directory
				//this lab uses the spring-petclinic application	
            			git 'https://github.com/admiralappsec/spring-petclinic-FIXED.git'   
    	    			}
    	 	}
    	    
		stage('Build') {
			steps{
				//skipping maven tests for demo purposes
				sh 'mvn clean package -DskipTests'
				sleep 10
			}
		}
     
		stage('Deploy Contrast Agent'){
			steps{
			contrastAgent agentType:'JAVA', profile:'<YOUR_PROFILE_CONFIGURATION_IN_JENKINS>',outputDirectory:"${pwd()}"
			}
		}
		    
		stage('Run Application') {
            		steps{
                sh 'java -javaagent:contrast.jar -Dcontrast.server=PetclinicPipelineSCMServer -Dcontrast.override.appversion=${JOB_NAME}-${BUILD_NUMBER} -Dcontrast.application.session_metadata="buildNumber=${BUILD_NUMBER}" -Dcontrast.standalone.appname=PetclinicPipelineSCM -jar target/*.jar &'
                sh 'sleep 75'
            		}
        	}
        
		stage('Hit an Endpoint') {
            		steps{
                		sh 'wget http://localhost:8081/owners?lastName=Smith'
                		sh 'sleep 20'
            		}
        	}
		    
        	stage('Contrast Verification') {
			steps{
			// use application ID from Contrast UI
            		// contrastVerification - this step verifies vulnerabilities with the Contrast Security Team Server
			contrastVerification applicationId: '895f532d-2346-4c86-a4e5-f62e7634838f', profile:'rstatsingerEval2',queryBy:3,count:0,severity:'High'
        		}
		}
        }
}

