pipeline {
	    agent any
	
	    parameters {
	         string(name: 'tomcat_dev', defaultValue: '172.31.9.121', description: 'Staging Server')
	         string(name: 'tomcat_prod', defaultValue: '172.31.10.165', description: 'Production Server')
	    }
	
	    triggers {
	         pollSCM('* * * * *')
	     }
	
	stages{
	        stage('Build'){
	            steps {
	                sh 'mvn clean package'
	            }
	            post {
	                success {
	                    echo 'Now Archiving...'
	                    archiveArtifacts artifacts: '**/target/*.war'
	                }
	            }
	        }
	
	        stage ('Deployments'){
	            parallel{
	                stage ('Deploy to Dev'){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/Package/target/vprofile-v1.war jenkins@${params.tomcat_dev}:/usr/local/apache-tomcat-8.5.31/webapps"
	                    }
	                }
	
	                stage ("Deploy to Production"){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/Package/target/vprofile-v1.war.war jenkins@${params.tomcat_prod}:/usr/local/apache-tomcat-8.5.31/webapps"
	                    }
	                }
	            }
	        }
	    }
	}
