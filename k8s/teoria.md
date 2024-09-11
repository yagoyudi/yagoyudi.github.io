# Kubernetes

O Kubernetes (k8s) é um orquestrador de containers.

Sumário:

1.  [Arquitetura do Kubernetes](#arquitetura-do-kubernetes)
1.  [Criação do cluster](#criação-do-cluster) TODO
1.	[Namespaces](#namespaces)
1.  [Pods](#pods)
1.  [Deployments](#deployments)
1.  [DaemonSets](#daemonsets) TODO
1.  [StatefulSets](#statefulsets) TODO
1.  [Jobs e Cronjobs](#jobs-e-cronjobs) TODO
1.  [Services](#services)
1.  [Ingress](#ingress) TODO
1.  [ConfigMap e Secrets](#ingress-e-secrets) TODO
1.  [Storage](#storage) TODO
1.  [Autoscaling](#autoscaling) TODO
1.  [Networking](#networking) TODO
1.  [RBAC](#rbac) TODO
1.  [Upgrade](#upgrade) TODO
1.  [Scheduling](#scheduling) TODO
1.  [Troubleshooting](#troubleshooting)
1.  [Referências](#referências)

## Arquitetura do Kubernetes

![Arquitetura do Kubernetes imagem](components-of-kubernetes.svg)

O Kubernetes é composto pelo:

1. Control plane
2. Data plane

### Control plane

#### etcd

Banco de dados do Kubernetes.

#### kube-apiserver

Faz o controle de tudo.

O Kubernetes nada mais é do que uma API.

#### kube-scheduler

Decide em qual node vai colocar um determinado pod.

#### kube-controller-manager

Todos os controles do Kubernetes compilador juntos.

Node controller, Job controller, EndpointSlice contoller, ServiceAccount
controller, etc.

#### cloud-controller-manager

Ponte de comunicação para a API de alguma Cloud.

### Data plane

O Data plane é composto pelos Nodes, que possuem os seguintes serviços:

#### kubelet

Garante que os containers estão rodando no Pod.

Comunica-se com o kube-apiserver.

#### kube-proxy

É um container que atua como proxy de rede.

Ele que cria o service.

#### container runtime

Responsável por garantir a execução do container e gerenciar o seu ciclo de
vida.

## Criação do cluster

### kind

Com o kind, você consegue um cluster local a partir de um yaml.

Exemplo:

```yaml
# config.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/valpha4

nodes:
- role: control-plane
- role: worker
- role: worker
- role: worker
```

Para aplicar essa configuração, usa o `kind create cluster --config config.yaml`.

### kubeadm

<https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/>

No controle plane:

```
kubeadm init --apiserver-advertise-address [IP da bridge] --pod-network-cidr [CIDR do seu CNI]

export KUBECONFIG=/etc/kubernetes/admin.conf
```

O coredns não sobe sem antes ter um CNI.

Como instalar o CNI (Flannel, Cillium, etc): `kubectl apply -f [arquivo yaml do seu CNI]`

### proxmox + terraform + kubespray

#### Provisionando o ambiente no Proxmox usando Terraform

#### Criando o cluster HA com Kubespray

<https://github.com/kubernetes-sigs/kubespray>

## Namespaces

São úteis para separar os pods.

### Comandos namespaces

* 	`k get ns`

	Lista as namespaces existentes.

* 	`k api-resources`

	Lista as keywords do k8s.

* 	`k create ns teste`

	Cria uma namespace chamada teste.

* 	`k get ns teste --dry-run -oyaml`

	Retorna:

	```yaml
	apiVersion: v1
	kind: Namespace
	metadata:
	  name: teste
	```

## Pods

Os Pods são a estrutura mais básica dentro da estrutura do Kubernetes.

A sua aplicação sempre vai rodar dentro do um pod.

Na grande maioria dos casos, o pod é um container. No entanto, pode ter um
container de proxy ou um sidecar também, por exemplo.

*Cuidado: O nome e o IP do pod sempre mudam.*

### Comandos pods

**k = kubectl**

*   `k run --image nginx nginx`

    Cria um pod com a imagem do nginx (do DockerHub).

*   `k run --image alpine --rm -it demo sh`

    Cria um pod com a imagem do alpine (do DockerHub), abre uma shell do pod e
    deleta o pod quando sair dele.

    Muito útil para debbugar a rede.

    `apk add curl bind-tools` instala algumas utilidades da rede no Alpine como
    o `ping`, `nslookup` e  etc.

*   `k exec -it nginx sh`

    Muito útil para debbugar o pod nginx.

*   `k delete po nginx`

    Deleta o pod do nginx.

*   `k get po`

    `k get po -A`

    `k get po -Aowide`

    Lista os pods rodando.

*   `k apply -f nginx-pod.yaml`

    Cria um pod a partir do arquivo `nginx-pod.yaml`

    ```yaml
    # nginx-pod.yaml
    apiVersion: v1
    kind: Pod
    metadata:
      creationTimestamp: null
      labels:
        run: nginx
      name: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
      dnsPolicy: ClusterFirst
      restartPolicy: Always
    status: {}
    ```

*   `k get po --show-labels`

    Mostra as labels de todos os pods.

*   `k logs po nginx`

    Pega os logs do pod nginx.

*   `k describe po nginx`

    Descreve o pod nginx.

*   `k get po -w`

    Executa o `watch` no `k get po`.

* 	`k run --image httpd multicontainer-pod --dry-run -oyaml | kubectl neat > pod-multicontainer.yaml`
	
	O --dryrun simula a execução do comando.

	O kubectl neat é um plugin do k8s que limpa o yaml.

### Init containers

São containers que servem para carregar o pod.

Por exemplo, quero que o container da minha aplicação suba somente depois de
que o container do banco de dados subiu. Nesse caso, você faz um init container
para subir o banco de dados.

Normalmente usam um container do busybox para fazerem init containers.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-cd
  namespace: default
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
      - containerPort: 80
  initContainers:
  - name: waitfordns
    image: busybox
    command:
      - sh
      - c
      - until nslookup mymysql; do echo "Trying to resolve..."; sleep 1; done
```

`k logs nginx-cd -c waitfordns` pega os logs do container waitfordns, que está
dentro do pod nginx-cd.

### Multi-containers

É um pod com mais de um container.

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: multicontainer-pod
  name: multicontainer-pod
spec:
  containers:
  - image: httpd
    name: httpd
  - image: alpine
    name: alpine
    command:
    - "sleep"
    - "9999999999999999"
```

Como o container do alpine está no mesmo pod que o do httpd, é possível dar um
`curl localhost` do container do alpine e ele vai conseguir chegar no webserver
do container httpd. Em resumo, os containers dentro de um mesmo pod possuem o
mesmo IP.

#### Como acessar um container dentro de pod sem "k exec"

1. Entre em algum dos nodes do cluster

2. `crictl ps` para listar o containers rodando

3. `crictl inspect [id do container] 2>/dev/null | jq '.info.pid'` para pegar o PID do container

4. `nsenter -t [pid] -n sh`

	nsenter é namespace enter.

	-t é o target.

	-n é para executar um comando dentro da namespace do processo.

### Static pods

Todos os pods do control plane são estáticos. Isso significa que mesmo que você
delete eles, eles serão recriados.

O kubelet sempre fica vendo se os statics pods estão rodando. Caso não esteja,
ele inicia.

Os static pods:
* estão atrelados a um node.
* não precisam de deployment.
* ficam em `/etc/kubernetes/manifest`.

*Cuidado: os static pods não escalam.*

### Pod lifecycle

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: worker-pod
  name: worker-pod
spec:
  terminationGracePeriodSeconds: 60
  containers:
  - image: alpine
    name: alpine
    command:
    - "sleep"
    - "999999999"
    lifecycle:
      preStop:
        exec:
          command:
          - sh
          - -c
          - curl 10.244.2.32
```

O terminationGracePeriodSeconds é o intervalo para o SIGTERM funcionar. Se
passar desse intervalo manda um SIGKILL. Se não especificar, o termination
grace period é de 30s.

O lifecycle são hooks do pod. São ações que você pode injetar quando o pod está
iniciando ou finalizando.

preStop é o que fazer antes de parar o container.

<https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/>

## Deployments

Usado para aplicações stateless.

O Deploy orquestra vários ReplicaSet e o ReplicaSet orquestra os Pods.

Existem 3 jeitos de colocar a aplicação no ar:
1. Deployment
2. StatefulSet
3. DaemonSet

Normalmente, o Deployment é o jeito de colocar a aplicação no ar mais usado.

### Comandos deployment

*   `k create deploy --image nginx nginx`

    Cria um deployment com a imagem do nginx.

*   `k get deploy`

    Lista os deploys.

*   `k scale deploy nginx --replicas 10`

    Cria 10 pods do nginx.

*   `k create deploy nginx --image nginx -oyaml | k neat`

    ```yaml
    kind: Deployment
    metadata:
      labels:
        app: nginx
      name: nginx
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: nginx
      template:
        metadata:
          creationTimestamp: null
          labels:
            app: nginx
        spec:
          containers:
          - image: nginx
            name: nginx
    ```

*   `k apply -f deploy.yaml`

    Faz o deploy a partir de um arquivo em yaml.

*   `k get rs`

    Lista as ReplicasSets.

*   `k edit deploy nginx`

    Edita o yaml do deploy do nginx e já aplica as mudanças.

*   `k scale deploy nginx --replicas 50`

    Altera o número de réplicas do deploy do nginx para 50.

    O correto é usar o Autoscalling ao invés de trocar o número de réplicas na
    mão.

### maxSurge e maxUnavailable

maxSurge: o maior número de pods que podem subir acima do número desejado de
pods.

maxUnavailable: o maior número de pods que podem estar unavailable durante um
update.

```yaml
kind: Deployment
metadata:
  labels:
    app: nginx
  name: nginx
spec:
  strategy:
    rollingUpdate:
      maxSurge: 10%
      maxUnavailable: 0
  replicas: 100
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
```

Para ver mais: `k explain deploy.spec.strategy.rollingUpdate`.

### Request e Limits

limits: define o máximo de recursos computacionais permitido usar.

requests: define o mínimo de recursos computacionais necessário.

`k explain deploy.spec.template.spec.containers.resources`

```yaml
kind: Deployment
metadata:
  labels:
    app: nginx
  name: nginx
spec:
  strategy:
    rollingUpdate:
      maxSurge: 10%
      maxUnavailable: 0
  replicas: 100
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        resources:
          requests:
            cpu: 100m
            memory: 512M
          limits:
            cpu: 10
            memory: 8G
```

## StatefulSet

## DaemonSet

Coloca um pod em cada node.

## Services

Tem o objetivo de expor o app.

### Comandos service

*   `k expose deploy webserver --port 80 --target-port 80`

    Expõe o deploy `webserver` na porta 80.

*   `k get svc webserver -oyaml`

    Retorna o yaml do serviço.

*   `k get ep webserver`

    Mostra todos os endpoints atrás do serviço do webserver.

## Misc

### Labels e Selectors

*   `k get po -l k8s-app=kube-dns -n kube-system`

    Seleciona os pods com a label k8s-app como kube-dns dentro do namespace
    kube-system.

### k rollout

Serve para voltar uma versão de um deploy, por exemplo.

*   `k rollout status deploy nginx`
*   `k rollout history deploy nginx`

    Lista as versões do deploy.

*   `k rollout undo deploy nginx`

    Volta para o antigo deploy do nginx do history.

### k top

*   `k top node`
    
    Igual ao comando top do linux.

## Trobleshooting

### k explain

*   `k explain deploy`

    Retorna tipo um manpage das opções disponíveis para o deployment na hora de
    escrever o yaml.

*   `k explain deploy.spec`

    Lista as opções disponíveis para o spec do deploy.

O explain pode ser usado em tudo do `k api-resources`.

## Referências

* https://kubernetes.io/docs/concepts/overview/components/


