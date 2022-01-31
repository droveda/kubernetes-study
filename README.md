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
* kubectl (Declarativo ou Imperativo) ferramenta utilizada para se comunicar com a API
  * Criar
  * Ler
  * Atualizar
  * Remover 

## Pods
* eh uma capsula, pode conter um ou mais containers
  * um pod ganha um endereco ip ex: 10.0.0.1 pode ter n containers em portas diferentes, ex: container1 -> 8080, container2 -> 9000
  * caso o container(s) falhe o pod vai ser removido e o kubernetes irá criar um novo pod (possívelmente com outro IP)
  * Containers dentro do pod compartilham o mesmo IP e podem se comunicar via **localhost**
  * Podem compartilhar volumes
  * Pod (10.0.0.1) <----> Pod (10.0.0.2)
  * Um pod é dado como encerrado quando todos os containers dentro dele param de funcionar

## Criando um pod (Maneira imperativa)
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
1. ClusterIP -> Possibilita a comunicação de diferentes pods dentro do mesmo cluster (Nao possibilita acesso de fora do cluster)
   1. ver exemplo no arquivo portal_noticias/svc-pod-2.yaml
2. NodePort -> Abre/Permite a comunicacao com o mundo externo, tb funciona como ClusterIP
   1. Para acesso externo deve ser utilizado o IP do node. Pode executar o comando: (kubectl get nodes -o wide) para obter o INTERNAL-IP no Linux
   2. No windows/mac com docker-desktop pode utilizar o 'localhost'
   3. ver exemplo no arquivo portal_noticias/svc-pod-1.yaml
3. LoadBalancer -> Utilizam automaticamente os balanceadores de carga de cloud providers, tb sao NodePort e ClusterIP
   1. Abre comunicação para o mundos externo usando o Load Balancer do provedor! (AWS, Google Cloud, Azure)
   2. ver exemplo no arquivo portal_noticias/svc-pod-1-loadbalancer.yaml

* kubectl get nodes -o wide (exibe os nodes)
* kubectl delete pods --all
* kubectl delete svc --all

## Labels
* !IMPORTANTE -> As labels servem para etiquetar nossos recursos, assim conseguimos dizer que um Pod com a label 'xxx' deve ser vinculada a um Service (ClusterIP por exemplo)
  * ver exemplo nos arquivos (pod-2.yaml e svc-pod-2.yaml)
* Através da Labels um service sabe quais pods ele deve gerenciar. (No pod usa 'label', no Service usa 'selector')

## ConfigMap
* Serve para guardar configuracoes, ex: variaveis de ambiente
* Permite reutilizacao e desacoplamento
* kubectl get configmap
* kubectl describe configmap db-configmap

# Kubernetes Course 2
## ReplicaSets
* Estrutura que pode encapsular um ou mais pods
* Posibilitar ter N replicas de um pod, deixando a app sempre disponivel
  * arquivo exemplo: portal-noticias-replicaset.yaml
* user+password sistema: admin/admin
* kubectl get rs

## Deployments
* Camada acima de um replicaset
* ao se definir um deployment estamos automaticamente definindo um replicaset
* permite gerenciamento e controle de versao das alteracoes
* arquivo de exemplo: nginx-deployment.yaml
* kubectl apply -f nginx-deployment.yaml
* kubectl get deployments
* kubectl rollout history deployment nginx-deployment
* kubectl apply -f nginx-deployment.yaml --record
* kubectl rollout history deployment nginx-deployment
* kubectl annotate deployment nginx-deployment kubernetes.io/change-cause="definindo a imagem com versao latest"
* kubectl rollout history deployment nginx-deployment
* kubectl rollout undo deployment nginx-deployment --to-revision=1

## Ajustando o sistema para usar Deployments
* Arrumando a casa
  * kubectl delete deployment nginx-deployment
  * kubectl delete -f portal-noticias-replicaset.yaml
* kubectl apply -f portal-noticias-deployment.yaml 
* kubectl delete pod sistema-noticias
* kubectl apply -f sistema-noticias-deployment.yaml
* kubectl delete pod db-noticias
* kubectl apply -f db-noticias-deployment.yaml

## Persistindo dados com o Kubernetes
* Volumes
  * hostPath
    * monta um volume similar aos volumes do docker
  * Configurar o volume no docker desktop, Settings -> Resources -> File Sharing
  * criar arquivo pod-volume.yaml
  * kubectl apply -f pod-volume.yaml
  * kubectl exec -it pod-volume --container nginx-container -- bash
    * testar ver se o volume foi criado e se esta tudo ok

## StatefulSet
* create file: sistema-noticias-statefulset.yaml
* kubectl apply -f sistema-noticias-statefulset.yaml
* kubectl get deployments
* delete deployments sistema-noticias-deployment

* criar arquivos: imagens-pvc.yaml, sessao-pvc-yaml
* atualizar o arquivo sistema-noticias-statefulset.yaml
* kubectl apply -f imagens-pvc.yaml 
* kubectl apply -f sessao-pvc.yaml 
* kubectl get pvc
* kubectl get pv
* kubectl get sc
* kubectl delete -f sistema-noticias-statefulset.yaml

## Probes (Livenes probe)
* Livenes Probe
  * see file portal-noticias-deployment.yaml (livenessProbe)
* Readiness Probe
  *  * see file portal-noticias-deployment.yaml (readinessProbe)

## Horizontal Pod Autoscaler
* see file portal-noticias-hpa.yaml and portal-noticias-deployment.yaml
* kubectl apply -f portal-noticias-deployment.yaml 
* kubectl apply -f portal-noticias-hpa.yaml
* kubectl get hpa

