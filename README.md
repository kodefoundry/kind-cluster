# Creating a kind-cluster
A documentation of my experience with creating a kubernetes cluster on my Mac using [kind](https://kind.sigs.k8s.io/ "kind")

Kind installation on Mac was easy and smooth using brew brew install kind

Let's try creating a 'Hello World' kind of cluster. As we know that kind uses docker containers to create nodes, I am interested to know about the size of the images. A quick peek at their docker hub repository gives us the necessary informations

|Image Name   | OS  | Size  |
| ------------ | ------------ | ------------ |
|kindest/base|ubuntu|~108 MB
|kindest/node|ubuntu|~458 MB


It would be interesting to see if these images can be build out of alpine, I am sure there would have been some issues or else these folks would have done it themselves. But that is a project for another day !!

lets get our hands dirty a little

Creating a cluster
`kind create cluster --name efk-basic`

Checking the cluster
`kubectl cluster-info --context kind-efk-basic`

	Kubernetes master is running at https://127.0.0.1:53382
	CoreDNS is running at https://127.0.0.1:53382/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

Get cluster details
`kind get clusters`

Deleting clusters
`kind delete cluster`

Docker images can be loaded easily to your cluster 
`kind load docker-image my-custom-image-0 my-custom-image-1 --name my-cluster-name`

Thats a cool feature so your development process will look like

	docker build ...
	kind load ...
	kubectl apply ...

Ok, we are able to bring up a simple cluster using kind, now I am interested to see if the cluster can seamlessly work with [helm](https://helm.sh/ "helm"). Obviously helm should be installed
`brew install helm`

I created a simple example helm chart following [this blog](https://opensource.com/article/20/5/helm-charts "this blog"), it worked like a charm.

Summerising what we have done till now.

1. We tried out [kind](https://kind.sigs.k8s.io/ "kind"), and created a simple cluster
2. We verified if [helm3](https://helm.sh/ "helm") is working with this cluster using a [hello-world](https://github.com/kodefoundry/kind-cluster/tree/main/hello-world "hello-world") available in the repository.

[========]

# Deploying a multinode K8s cluster and deploy a sample application, EFK and Nginx Ingress

The plan

	Create a multinode cluster
	Deploy Nginx Ingress
	Deploy a REST API backend server
	Deploy EFK
	Configure EFK to monitor logs

Multinode cluster was sucessfully created using the [kind.yaml](https://github.com/kodefoundry/kind-cluster/blob/main/kind.yaml "kind.yaml") available in the repository. Just run the below command

`kind create cluster --config kind.yaml
 kind delete cluster --name rnd-cluster`

Nginx ingress was deployed gracefuly using helm 

    helm repo add nginx-stable https://helm.nginx.com/stable
    helm repo update
    helm install nginx-release nginx-stable/nginx-ingress

I created a simple [REST API microservice](https://github.com/kodefoundry/api-service "REST API microservice") and created helm chart for same and deployed it into the cluster. Please go through the readme sections in the main project and under deploy for details.

