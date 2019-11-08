pipeline {
	 agent any
    	tools {
        	jdk 'jdk8'
        	maven 'maven3'
    	}

    	stages {
		stage ('Clone the project') {
    	    		steps{
            		git 'https://github.com/rstatsinger/spring-petclinic.git'   
    	    		}
    	 	}
    	    
		stage('Build') {
			steps{
				sh 'mvn clean package -DskipTests'
				sleep 10
			}
		}
     
		stage('Deploy Contrast Agent'){
			steps{
			contrastAgent agentType:'JAVA', profile:'rstatsingerEval2',outputDirectory:"${pwd()}"
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
                		sh 'sleep 10'
            		}
        	}
		    
        	stage('Contrast Verification') {
			steps{
			// use application ID from Contrast UI 895f532d-2346-4c86-a4e5-f62e7634838f
            		// contrastVerification profile:'rstatsingerEval2',applicationName:'PetclinicPipelineSCM',count:0,severity:'High'
			contrastVerification applicationId: '895f532d-2346-4c86-a4e5-f62e7634838f', profile:'rstatsingerEval2',queryBy:4,appVersionTag=${JOB_NAME}-${BUILD_NUMBER},count:0,severity:'High'
        		}
		}
        }
}

