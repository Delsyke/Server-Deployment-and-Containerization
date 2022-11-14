prerequisites - docker, eksctl, awscli, kubectl
check kubectl client version
login to docker


get caller id 
	`aws sts get-caller-identity --query Account --output text`
	replace value in trust.json file

IAM configuration
ensure aws-session-token = ""
aws iam create-role --role-name UdacityFlaskDeployCBKubectlRole --assume-role-policy-document file://trust.json --output text --query 'Role.Arn'

create cluster =====> 
	`eksctl create cluster --name clustername --nodes=2 --version=1.2X --instance-types=t2.medium --region=us-east-2`

	ensure the version is close to your kubectl client version
	Here kubectl will be configured automatically with the clusters

	check health of nodes ======>
		`kubectl get nodes`



build docker image and push to docker hub


deploy the app the cluster ====>
	check your deployment settings in the yml file
	confirm pods are still health, running and ready
	`kubectl apply -f deployment.yml`

	other useful commands =====>
		* Verify the deployment *
		`kubectl get deployments`

		* Check the rollout status *
		`kubectl rollout status deployment/simple-flask-deployment`

		* IMPORTANT: Show the service, nodes, and pods in the cluster *
		* You will notice that the service does not have an external IP*
		`kubectl get svc,nodes,pods`

		* Show the services in the cluster *
		`kubectl describe services`

		* Display information about the cluster *
		`kubectl cluster-info`


expose the service to access the application
	`kubectl expose deployment simple-flask-deployment --type=LoadBalancer --name=my-service`


access service through external ip:port (port specified in yml file)

