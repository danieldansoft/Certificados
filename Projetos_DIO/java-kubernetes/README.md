## :books:Projetos Concluídos:books:

### *Daniel Cerri Ribeiro*

##   <img src="img/eu_lkdn.jpg" alt="0" style="zoom: 50%;" />

#### **Olá bem vindo... aos meus Projeto do BootCamp** 

**Projetos realizados BootCamp Inter Developer** :computer:

-----------------------------------------------------

- _**[Digital Inovation One](https://web.digitalinnovation.one/)**_
- **BootCamp Inter Developer**

-----------------------------------------------------

`copyright © Daniel 2021`

**Instruções abaixo**





# Java e Kubernetes

Mostre como você pode mover seu aplicativo de inicialização rápida para o docker e o kubernetes.
Este projeto é uma demonstração para a série de postagens em dev.to
https://dev.to/sandrogiacom/kubernetes-for-java-developers-setup-41nk

## Parte um - aplicativo básico:

### Requisitos:

** Docker e Make (opcional) **

** Java 15 **

Ajuda para instalar ferramentas:

https://github.com/sandrogiacom/k8s

### Construir e executar o aplicativo:

Spring boot e banco de dados mysql em execução no docker

** Clone do repositório **
`` `bash
git clone https://github.com/sandrogiacom/java-kubernetes.git
`` `

** Crie um aplicativo **
`` `bash
cd java-kubernetes
mvn clean install
`` `

** Inicie o banco de dados **
`` `bash
make run-db
`` `

** Execute o aplicativo **
`` `bash
java --enable-preview -jar target / java-kubernetes.jar
`` `

**Verificar**

http: // localhost: 8080 / app / users

http: // localhost: 8080 / app / hello

## Parte dois - aplicativo no Docker:

Crie um Dockerfile:

`` `yaml
DE openjdk: 15-alpine
RUN mkdir / usr / myapp
COPY target / java-kubernetes.jar /usr/myapp/app.jar
WORKDIR / usr / myapp
EXPOSE 8080
ENTRYPOINT ["sh", "-c", "java --enable-preview $ JAVA_OPTS -jar app.jar"]
`` `

** Crie um aplicativo e uma imagem docker **

`` `bash
fazer construir
`` `

Crie e execute o banco de dados
`` `bash
make run-db
`` `

Crie e execute o aplicativo
`` `bash
make run-app
`` `

**Verificar**

http: // localhost: 8080 / app / users

http: // localhost: 8080 / app / hello

Pare tudo:

`
docker stop mysql57 myapp
`

## Parte três - aplicativo no Kubernetes:

Temos um aplicativo e uma imagem em execução no docker
Agora, implantamos o aplicativo em um cluster Kubernetes em execução em nossa máquina

Preparar

### Iniciar minikube
`
make k-setup
`
 inicie o minikube, ative o ingresso e crie o namespace dev-to

### Verifique o IP

`
minikube -p dev.to ip
`

### Painel do Minikube

`
painel de controle minikube -p dev.to
`

### Implantar banco de dados

criar implantação e serviço mysql

`
make k-deploy-db
`

`
kubectl get pods -n dev-to
`

OU

`
assistir k obter pods -n dev-to
`


`
kubectl logs -n dev-to -f <pod_name>
`

`
kubectl port-forward -n dev-to <pod_name> 3306: 3306
`

## Construir aplicativo e implantar

construir aplicativo

`
make k-build-app
`

criar imagem docker dentro da máquina minikube:

`
fazer k-build-image
`

OU

`
fazer k-cache-image
`

criar implantação de aplicativo e serviço:

`
make k-deploy-app
`

**Verificar**

`
kubectl get services -n dev-to
`

Para acessar o aplicativo:

`
minikube -p dev.to service -n dev-to myapp --url
`

Ex:

http://172.17.0.3:32594/app/users
http://172.17.0.3:32594/app/hello

## Verifique os pods

`
kubectl get pods -n dev-to
`

`
kubectl -n dev-to logs myapp-6ccb69fcbc-rqkpx
`

## Mapear para dev.local

obter IP minikube
`
minikube -p dev.to ip
`

Editar `hosts`

`
sudo vim / etc / hosts
`

Réplicas
`
kubectl get rs -n dev-to
`

Obter e excluir pod
`
kubectl get pods -n dev-to
`

`
kubectl delete pod -n dev-to myapp-f6774f497-82w4r
`

Escala
`
kubectl -n dev-to scale deployment / myapp --replicas = 2
`

Réplicas de teste
`
enquanto verdadeiro
do curl "http: //dev.local/app/hello"
eco
dormir 2
feito
`
Teste as réplicas com espera

`
enquanto verdadeiro
do curl "http: //dev.local/app/wait"
eco
feito
`

## Verifique o url do aplicativo
`minikube -p dev.to service -n dev-to myapp --url`

Altere seu IP e PORTA conforme necessário

`
curl -X GET http: //dev.local/app/users
`

Adicionar novo usuário
`
curl --location --request POST 'http: //dev.local/app/users' \
--header 'Content-Type: application / json' \
--data-raw '{
    "nome": "novo usuário",
    "birthDate": "2010-10-01"
} '
`

## Parte quatro - aplicativo de depuração:

adicionar JAVA_OPTS: "-agentlib: jdwp = transport = dt_socket, address = *: 5005, server = y, suspend = n"

alterar CMD para ENTRYPOINT no Dockerfile

`
kubectl get pods -n = dev-to
`

`
kubectl port-forward -n = dev-to <pod_name> 5005: 5005
`

## KubeNs e Stern

`
Kubens Dev-to
`

`
popa myapp
`

## Começar tudo

`make k: all`


## Referências

https://kubernetes.io/docs/home/

https://minikube.sigs.k8s.io/docs/