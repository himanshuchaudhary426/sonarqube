pipeline{
	agent any
	stages{
	    stage('SCM'){
	        steps{
	            git credentialsId: 'git-personal', url: 'https://github.com/himanshuchaudhary426/springbootproject.git'
	        }
	    }
		stage('Test'){
			steps{
				withMaven(jdk: 'java8', maven: 'Maven3') {
    				sh "mvn test"
				}
			}
		}
		stage('sonarqube'){
		    steps{
		        script{
		            withSonarQubeEnv('sonarqube') {
                        sh "mvn sonar:sonar"
		            }
                        timeout(time: 1, unit: 'HOURS') {
                            def qg = waitForQualityGate()
                            if(qg.status == 'OK'){
                                error "pipeline aborted due to quality gate failure: "$(qg.status)
                            }
                        }
			    }
		    }
		}
		stage('packaging')
		{
			when{
				branch 'master'
			}
			steps{
				withMaven(jdk: 'java8', maven: 'Maven3') {
    				sh "mvn -DskipTests package"
				}
			}
		}
	}
}
