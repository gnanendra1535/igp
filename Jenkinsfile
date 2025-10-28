pipeline
{
	agent any
	stages
	{
		stage('Code Checkout')
		{
			steps
			{
				git 'https://github.com/gnanendra1535/igpoct2025.git'
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
					sh 'docker build -t gnanendrame/abc_tech:$BUILD_NUMBER .'
			}
		}

		stage('Push Docker Image')
		{ 
			steps
			{   
			    withDockerRegistry([ credentialsId: "dockerhub", url: "" ])
			    {
			       sh 'docker push gnanendrame/abc_tech:$BUILD_NUMBER'
			    }
			}
		}

		stage('Deploy as container')
		{
			steps
			{
				sh 'docker run -d -P gnanendrame/abc_tech:$BUILD_NUMBER'
			}
		}

   }
}