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

## Kubernetes Basics
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

## Pods
* eh uma capsula, pode conter um ou mais containers
  * um pod ganha um endereco ip ex: 10.0.0.1 pode ter n containers em portas diferentes, ex: container1 -> 8080, container2 -> 9000

## Criando um pod
* kubectl run nginx-pod --image=nginx:latest
* kubectl get pods
* kubectl get pods --watch
* kubectl describe pod nginx-pod
* Changing a pod
  * kubectl edit pod nginx-pod

## Criando um pod de maneira declarativa (Visual Studio Code install kubernetes extensions) 
* create the file: /definition/primeiro-pod.yaml
* kubectl apply -f primeiro-pod.yaml
* kubectl get pods
* kubectl delete pod nginx-pod
* kubectl delete -f primeiro-pod.yaml

## Creating an app
* kubectl apply -f portal-noticias.yaml 
* kubectl get pods --watch
* kubectl describe pod portal-noticias (with this command we can see for example the pod IP and other infos)
* kubectl exec -it portal-noticias -- bash
* kubectl get pods -o wide
* kubectl apply -f svc-pod-2.yaml
* kubectl get svc (Exibe os services)

## SVC - Service
* Abstracoes para expor aplicacoes executando em um ou mais pods
* Proveem IP's fixos para comunicacao
* Provem um DNS para um ou mais pods
* Sao capazes de fazer balancemamento de carga
1. ClusterIP
2. NodePort -> Abre/Permite a comunicacao com o mundo externo, tb funciona como ClusterIPs
3. LoadBalancer -> Utilizam automaticamente os balanceadores de carga de cloud providers, tb sao NodePort e ClusterIP

* kubectl get nodes -o wide (exibe os nodes)
* kubectl delete pods --all
* kubectl delete svc --all

## ConfigMap
* Serve para guardar configuracoes, ex: variaveis de ambiente
* Permite reutilizacao e desacoplamento
* kubectl get configmap
* kubectl describe configmap db-configmap