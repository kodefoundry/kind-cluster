# Creating a kind-cluster
A documentation of my experience with creating a kubernetes cluster on my Mac using kind (kind.sigs.k8s.io)

Kind installation on Mac was easy and smooth using brew
  brew install kind

Let's try creating a 'Hello World' kind ( ;) )  of cluster. As we know that kind uses docker containers to create nodes. I am interested to know about the size of the images. A quick peek at their docker hub repository gives us the necessary informations
