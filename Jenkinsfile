pipeline{
    agent any
    environment{
        PATH = "/opt/apache-maven-3.8.2/bin:$PATH"
    }
    stages{
        stage('Git checkout'){
            steps{
               git credentialsId: 'github', url: 'https://github.com/Ram9500/my-app1'
            }
        }
	    stage ('maven build'){
		    steps{
			    sh 'mvn clean package'
			      sh 'mv target/myweb*.war target/newapp.war'
		    }
	    }
	    stage('deployment-dev'){
		    steps{
		 	  sshagent(['tomcat']) {
                             sh """
                               scp -o StrictHostKeyChecking=no target/newapp.war ec2-user@172.31.37.54:/opt/tomcat8/webapps/
		               ssh ec2-user@172.31.37.54 /opt/tomcat8/bin/shutdown.sh
		               ssh ec2-user@172.31.37.54 /opt/tomcat8/bin/startup.sh
                                """
                      }
	           }
              }
	    stage('Email notification'){
		    
		    steps{
			    mail bcc: '', body: '''Hi welcome to Jenkins email alerts
                            Thanks 
                            Ram''', cc: '', from: '', replyTo: '', subject: 'Jenkins-job', to: 'ramanujam96cloud@gmail.com'
		    }
	    }
	    stage('slack Notification'){
		    steps{
		    slackSend baseUrl: 'https://hooks.slack.com/services/', 
			    channel: 'jenkins-pipeline', 
			    color: 'good', 
			    message: 'Welcome to jenkins slack!', 
			    tokenCredentialId: 'slack-demo', 
			    username: 'spatialcorp'
		    }
	    }
        }
    }
