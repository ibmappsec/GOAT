pipeline {
    agent any
    stages {
	stage('Pull project code'){
		steps {
			echo 'downloading git directory..'
			git 'https://github.com/ibmappsec/GOAT.git'
		}
	}
	stage('Prepare SCA tool'){
		steps {
			script{				
				def exists = fileExists 'dependency-check-5.2.1-release.zip'
				if(exists){
					echo 'already exists'
				}else{
					sh 'wget https://dl.bintray.com/jeremy-long/owasp/dependency-check-5.2.1-release.zip'
					sh 'unzip dependency-check-5.2.1-release.zip'				
				}
			}
		}
	}
	stage('Package project') {
            steps {
		    script{
			echo 'Packing maven project'
			    if(!fileExists('webgoat-server-8.0.0.M25.jar')){
				sh 'wget https://github.com/WebGoat/WebGoat/releases/download/v8.0.0.M25/webgoat-server-8.0.0.M25.jar'		    
			    }
		    }
            }
        }
	stage('Run dependency check'){
		steps {
			echo 'running dependency check on java jar file'
			sh './dependency-check/bin/dependency-check.sh --project Testing --out m25.html --scan *.jar -f HTML --failOnCVSS 7'
			sh './dependency-check/bin/dependency-check.sh --project Testing --out webgoat.html --scan ./webgoat-server/target/*.jar.* -f HTML --failOnCVSS 7'
			sh './dependency-check/bin/dependency-check.sh --project Testing --out webwolf.html --scan ./webwolf/target/*.jar.* -f HTML --failOnCVSS 7'
		}
	}
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
