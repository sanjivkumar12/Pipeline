pipeline {
    agent any
   
 parameters {
        string(name: 'RELEASE_VERSION', defaultValue: '1.0.0', description: 'Application git release tag version')
    }
    environment { 
        ENV_STACK = 'staging'
    }  

    stages 
    {
        stage('No-op') 
	{
            steps {
                echo "Hello World!"
                echo "echo Hello from the shell"
                echo "hostname"
                echo "uptime"
            	}
	}

stage('Clone sources') {
            steps{
        git url: 'https://github.com/sanjivkumar12/Pipeline.git'
        echo "Cloned Successfully"
            }
    }	
    
stage('ExampleUsingVariables') {
            steps {
                echo "Deploying ${params.RELEASE_VERSION} in ${env.ENV_STACK}"
            }
        } 

    }       

    post {
        always {
            echo 'One way or another, I have finished'
            deleteDir() /* clean up workspace */
            
            script{
	try {
		if('SUCCESS' != currentBuild.getPreviousBuild().getResult()) {
			mail (to: 'SANJIV.VERMA@wipro.com', subject: "${env.JOB_NAME}' (${env.BUILD_NUMBER}) - Back to normal", body: "Build back to normal: ${env.BUILD_URL}.");
		} else {
			mail (to: 'SANJIV.VERMA@wipro.com', subject: "${env.JOB_NAME}' (${env.BUILD_NUMBER}) - Back success", body: "Build success: ${env.BUILD_URL}.");
		}
	} catch (e) {
		mail (to: 'SANJIV.VERMA@wipro.com', subject: "${env.JOB_NAME}' (${env.BUILD_NUMBER}) - Failure!", body: "Build failed ${env.BUILD_URL}.");
	}
}
        }
        success {
            echo 'I succeeeded!'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'I failed :('
        }
        changed {
            echo 'Things were different before...'
        }
    }
}
