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
			contrastAgent agentType:'JAVA', profile:'rstatsingerEval',outputDirectory:"${pwd()}"
			}
		}
		    
		stage('Run Application') {
            		steps{
                sh 'java -javaagent:contrast.jar -Dcontrast.server=PetclinicPipelineSCMServer -Dcontrast.override.appversion=${JOB_NAME}-${BUILD_NUMBER} -Dcontrast.standalone.appname=PetclinicPipelineSCM -jar target/*.jar &'
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
            		contrastVerification profile:'rstatsingerEval',applicationName:'PetclinicPipeline',count:0,severity:'High'
        		}
		}
        }
}

