pipeline {
	agent any
	//this pipeline job assumes you have java jdk8 and maven3 configured
    	tools {
        	jdk 'jdk8'
        	maven 'maven3'
    	}
	//pipeline stages include: 'Clone the project', 'Build', 'Download Contrast Agent', 'Run Application', 'Hit an EndPoint', 'Contrast Verification'
	//You an also use the Contrast Maven plugin within the 'Build' step to take advantage of the Contrast Security Platform capability within your maven builds
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
				//****You an also use the Contrast Maven plugin within the 'Build' step to take advantage of the Contrast Security Platform capability within your maven builds
				sh 'mvn clean package -DskipTests'
				//wait 10 seconds
				sleep 10
			}
		}
		stage('Download Contrast Agent'){
			steps{
				//this step downloads the latest Contrast Security Agent into your Jenkins workspace
				//the petclinic application is a java application, so we explicitly pass 'JAVA' into our agent type
				contrastAgent agentType:'JAVA', profile:'<YOUR_PROFILE_CONFIGURATION_IN_JENKINS>',outputDirectory:"${pwd()}"
			}
		}   
		stage('Run Application') {
            		steps{
				//this step deploys the petclinic application along with the Contrast Security Agent instrumentation
				//agent instrumentation and usage can be found at https://docs.contrastsecurity.com/en/install-the-java-agent.html
                		sh 'nohup java -javaagent:<CONTRAST.JAR_LOCATION> -Dcontrast.server=<YOUR_CONTRAST_TEAM_SERVER_URL> -Dcontrast.override.appversion=${JOB_NAME}-${BUILD_NUMBER} -Dcontrast.application.session_metadata="buildNumber=${BUILD_NUMBER}" -Dcontrast.standalone.appname=<CONTRAST_SECURITY_APPLICATION_NAME> =8081 -jar target/*.jar >> <JENKINS_LOG_LOCATION>&'
                		//wait 75 seconds
				sh 'sleep 75'
            		}
        	}
		stage('Hit an Endpoint') {
            		steps{
				//simulate hitting an endpoint within the application
                		sh 'wget http://localhost:8081/owners?lastName=Smith'
				//wait 20 seconds
                		sh 'sleep 20'
            		}
        	}  
        	stage('Contrast Verification') {
			steps{
				//use application ID from Contrast UI
            			//contrastVerification - this step verifies vulnerabilities with the Contrast Security Team Server
				//queryBy,count, and severity are all configuration parameters passed to the contrastVerfication step - usage can be found at https://docs.contrastsecurity.com/en/jenkins.html
				contrastVerification applicationId: '<YOUR_CONTRAST_SECURITY_APPLICATION_ID_FROM_CS_TEAM_SERVER>', profile:'<YOUR_PROFILE_CONFIGURATION_IN_JENKINS>',queryBy:3,count:0,severity:'High'
        		}
		}
        }
}

