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
                echo "Hello, from the No-op stage"
            	}
	}

stage('Clone Repo') {
            steps{
        git url: 'https://github.com/sanjivkumar12/Pipeline.git'
        echo "Cloned repo Successfully"
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
