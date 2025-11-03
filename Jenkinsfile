pipeline
{
	agent any
	stages
	{
			stage('Code Checkout')
		{
			steps
			{
				git branch: 'main',
				    url: 'https://github.com/gnanendra1535/igp.git'
			}
		}
		
		stage('Code Compile')
		{
			steps
			{
				sh 'mvn compile'
			}
		}

		stage('Unit Test')
		{
			steps
			{
				sh 'mvn test'
			}
		}

		stage('Code packaging')
		{
			steps
			{
				sh 'mvn package'
			}
		}

		// stage('install docker using ansible')
		// {
		// 	steps
		// 	{
		// 		sh 'ansible-playbook docker.yaml'
		// 	}
		// }

		stage('Build Docker Image')
		{
			steps
			{
					sh 'docker build -t gnanim07/abc_tech:$BUILD_NUMBER .'
			}
		}

		stage('Push Docker Image')
		{ 
			steps
			{   
			    withDockerRegistry([ credentialsId: "gnani-dockerhub", url: "" ])
			    {
			       sh 'docker push gnanim07/abc_tech:$BUILD_NUMBER'
			    }
			}
		}

		stage('Deploy as container')
		{
			steps
			{
				sh 'docker run -d -P gnanim07/abc_tech:$BUILD_NUMBER'
			}
		}

		stage('Deploy to k8s cluster ')
		{
			steps
			{
			      withCredentials([file(credentialsId: 'eks-kubeconfig', variable: 'KUBECONFIG_FILE')]) {
          sh '''
            export KUBECONFIG=$KUBECONFIG_FILE
            kubectl version --client
            kubectl apply -f deploy.yaml --validate=false
            kubectl apply -f svc.yaml --validate=false
          '''
        }
			
			}
		}
   }
}
