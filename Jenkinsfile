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
	
	stage('Checkout Project properties') {
            steps {
		    
            bat 'mkdir -p Properties'
            dir("Properties")
		    
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], 
                doGenerateSubmoduleConfigurations: false, 
                extensions: [], 
                gitTool: 'default', 
                submoduleCfg: [], 
                            userRemoteConfigs: [[
                            credentialsId: 'GithubCredentials',
                            url: "$GIT_PROJECT_PROPERTIES"
                        ]]
                ])
            }
              
        }    
	stage('Checkout Project') {
            steps {
	    bat 'mkdir wefferent'
	    bat ' Icacls /wefferent /grant F'
	    bat 'cd wefferent'
                checkout([$class: 'GitSCM', branches: [[name: '/master']], 
                doGenerateSubmoduleConfigurations: false, 
                extensions: [], 
                gitTool: 'default', 
                submoduleCfg: [], 
                            userRemoteConfigs: [[
                            credentialsId: 'GithubCredentials',
                            url: "$GIT_PROJECT"
                        ]]
                ])
            }
              
        }
	

		stage('Build') {
		    steps {
			bat "./gradlew build"
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
			    
			    bat 'dir' 
			 withSonarQubeEnv('SonarQube') {
				 bat "${scannerHome}/bin/sonar-scanner -X -Dproject.settings=sonar-scanner.properties"
			}
		   }
		 }

	    }
	}  
