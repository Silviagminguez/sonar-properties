// Pipeline para proyectos Android
//
// V.1.0.0
//

def server_up = false

pipeline {
	environment{
		GIT_PROJECT = "https://github.com/Silviagminguez/sonar-local.git"
		GIT_PROJECT_PROPERTIES = "https://github.com/Silviagminguez/sonar-properties.git"
		scannerHome = tool 'SonarQubeScanner'
	}
	
	
agent any
	//    agent { label "sdk5" }
stages {
   
	 
	stage('Checkout Project') {
            steps {
           	//sh 'mkdir Repo1'
		    dir( 'Repo1'){
			    
		    
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], 
                doGenerateSubmoduleConfigurations: false, 
                extensions: [], 
                gitTool: 'default', 
                submoduleCfg: [], 
                            userRemoteConfigs: [[
                            credentialsId: 'GitCredentials',
                            url: "$GIT_PROJECT"
                        ]]
                ])
		    }
            }
              
        }
		stage('Checkout Project properties') {
            steps {
	       //sh 'mkdir Repo2'
		dir( 'Repo2'){
			    sh 'pwd'
		    
        
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], 
                doGenerateSubmoduleConfigurations: false, 
                extensions: [], 
                gitTool: 'default', 
                submoduleCfg: [], 
                            userRemoteConfigs: [[
                            credentialsId: 'GitCredentials',
                            url: "$GIT_PROJECT_PROPERTIES"
                        ]]
                ])
		}
		      
            }
	    
        } 
	
		stage('Build') {
		    steps {
			    script{
			    mv /Repo2/Jenkinsfile /Repo1
			    mv /Repo2/"sonar-scanner.properties" /Repo1
			    }
			sh "./gradlew assembleDebug"
			   
		    }
		}
		stage('Compile') {
		    steps {
		    archiveArtifacts artifacts: '**/*.apk', fingerprint: true, onlyIfSuccessful: true            
		    }
		}
		 stage ('sign apk') { 
		    steps{
		    signAndroidApks ( 
		    keyStoreId: "AndroidSign", 
		    keyAlias: "my-alias", 
		    apksToSign: "**/*-unsigned.apk" 
		    ) 
		    }   
		 }
		/* stage('Sonarqube') {
			environment {
			scannerHome = tool 'SonarQubeScanner'
			}
		    steps {
			 withSonarQubeEnv('SonarQube') {
			 bat "${scannerHome}/bin/sonar-scanner -X -Dproject.settings=sonar-scanner.properties "
			}
		   }
		 }*/
			
  

		    stage('Sonarqube') {
			
		    steps {
			    echo "estoy en***********************************"
			    
			  
			 withSonarQubeEnv('SonarQube') {
				sh "${scannerHome}/bin/sonar-scanner -X -Dproject.settings=sonar-scanner.properties"
			}
		   }
		 }

	    }
	}  
