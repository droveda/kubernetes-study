# Useful commands and instructions
## Configure vm using vagrant
* vagrant init ubuntu/bionic64
* edit the Vagrantfile following the example
* vagrant up
* Install and configure docker

## kubectl install
* sudo apt-get install curl -y
* curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
* chmod +x ./kubectl
* sudo mv ./kubectl /usr/local/bin/kubectl

## Minikube install
* curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
* sudo install minikube-linux-amd64 /usr/local/bin/minikube
* Install virtual box (download the .deb file) - I tried after without this and it worked
* cd Downloads/
* sudo dpkg -i "virtual box deb file".deb

## Another option for Mac m1 - (This one is very easy to do, minikube is not needed)
* first Install "Docker desktop" for mac m1 (dmg file - obs: This will install **kubeclt** as well)
* go to **preferences** mark "Enable Kubernetes" then click on **Apply and restart**
* to test it, open terminal and type: kubectl get nodes

## Minikube install Mac m1 - new (use the other option it is better)
* first Install "Docker desktop" for mac m1 (dmg file - obs: This will install **kubeclt** as well)
* curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-arm64
* sudo chmod +x minikube-darwin-arm64
* mv minikube-darwin-arm64 minikube
* sudo mv minikube /usr/local/bin
* to test it
  * minikube version
  * minikube --help
  * minikiube start  

## Kubernetes commands
* minikube start 
    * (--vm-driver=virtualbox, I ran without this and it worked) (start the minikube - (it is necessary do execute this command every time you start the SO))
    * https://stackoverflow.com/questions/61999850/minikube-external-ip-not-match-hosts-public-ip
    * https://github.com/kubernetes/minikube/issues/3966
* kubectl get nodes
* kubeclt get pods
* kubeclt delete pods --all
* kubectl apply -f portal-noticias.yaml
* kubectl apply -f svc-portal-noticias.yaml
* kubectl get nodes -o wide
* kubectl describe pod portal-noticias (check the creation logs of a pod - useful to check errors)
* kubectl get configmap

# Kubernetes Basics
* cluster (Master, nodes)
  * master (Control Plane -> (api, c-m, sched, etcd))
    * Gerenciar os cluster
    * Manter e atualizar o estado desejado
    * Receber e executar novos comandos
  * node (Nodes -> (Kubelet, K-Proxy))
    * Executar as aplicacoes
* pod = recurso que encapsula um container no kubernetes
* kubectl
  * Criar
  * Ler
  * Atualizar
  * Remover 