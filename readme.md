# Kind Install ☸ 👨‍💻 

**Link da documentação do kind Kubernetes**

Clique aqui - [Kind ✔️](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)




🔵 **OBS!** O kind e baseado no kubeadmin

🔵 **OBS!** Antes de criar um cluster com o kind certique-se de que o kubectl esteja instalado, caso não esteja, clique aqui [kubectl ✔️](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

**Ao entrar no site, digite os seguintes comandos para realizar a instalação do kubectl passo-a-passo**

1. Baixe a versão mais recente com o comando:

    ```vim
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    ```

2. **Valide o binário (Opcional)**
    ```vim 
    curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
    ```

    **Valide o binário kubectl no arquivo de soma de verificação:**
    ```vim
    echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
    ```

    Se válido, a saída é:

    > kubectl: 

    Se a verificação falhar, sha256 sairá com status diferente de zero e imprimirá uma saída semelhante a:

    > kubectl: FAILED
    >
    > sha256sum: WARNING: 1 computed checksum did NOT 
    
    > **Observação 📘:** Baixe a mesma versão do binário e da soma de verificação.

3. Instalar kubectl

    ```vim
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    ```

    > **Observação 📘:**
    >
    > Se você não tiver acesso root no sistema de destino, ainda poderá instalar o kubectl no diretório ~/.local/bin:
    >
    > ```shell
    > chmod +x kubectl
    > mkdir -p ~/.local/bin
    > mv ./kubectl ~/.local/bin/kubectl
    > # e então anexe (ou anexe) ~/.local/bin a $PATH
    > ```
    > ---

4. Teste para garantir que a versão que você instalou está atualizada:

    ```shell
    kubectl version --client
    ```

    > **Observação 📘:**
    >
    > O comando acima irá gerar um aviso:
    >
    >   ```WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.```
    >
    > Você pode ignorar este aviso. Você está verificando apenas a versão do ``kubectl`` que instalou.
    >
    > ---

    Ou use isso para uma visão detalhada da versão:

    ```shell
    kubectl version --client --output=yaml
    ```


**Use o comando para permitir execução de script para todos**

```vim
chmod a+x kubectl
```

**Depois mova o kubectl**

```vim
sudo mv kubectl /usr/local/bin
```


## Instalando o Kind
```vim

# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
# For ARM64
[ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-arm64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

```

**Para criar um cluster no kind digite o seguinte comando:**

```shel

kind create cluster

```

**Criando um cluster e inserindo um nome nele**

```shell
kind create cluster --name <nome-cluster>
```

**Ver todos os clusters**
```shell
kind get clusters
```

**Deletando cluster**

```shell
kind delete cluster

#ou

kind delete cluster --name <nome-cluster>
```

**Rode um comando docker para ver os containers de cada cluster rodando**

```shel
docker container ls
```

## Configuração do kind

Segue o link da documentação do kind para configuração do mesmo: [Configuração do kind ✔️](https://kind.sigs.k8s.io/docs/user/quick-start/#configuring-your-kind-cluster)

### Multi-node clusters

Em particular, muitos usuários podem estar interessados ​​em clusters de vários nós. Uma configuração simples para isso pode ser obtida com o seguinte conteúdo do arquivo de configuração:

```yaml
# three node (two workers) cluster config
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

🔵 **OBS!** Posso colocar mais de um node no cluster, mas não posso colocar mais de um control-plane

🔵 **OBS!** O kind não suporta o uso de nós de controle de vários nós, portanto, não é possível criar clusters HA com o kind

**Comando para instlar o cluster com a configuracao**

```shell
kind create cluster --name k8s-multinode --config config.yaml
```

### Control-plane HA

O que e control-plane HA?

> O control-plane HA é um cluster que possui vários nós de controle. Isso significa que, se um nó de controle falhar, o cluster ainda poderá ser gerenciado pelos outros nós de controle. Isso é diferente de um cluster de vários nós, que possui vários nós de trabalho, mas apenas um nó de controle. Se o nó de controle falhar, o cluster inteiro falhará.

```yaml

# a cluster with 3 control-plane nodes and 3 workers
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: control-plane
- role: control-plane
- role: worker
- role: worker
- role: worker
```

**Comando para deletar o K8s-multinode**

```shell
kind delete cluster --name k8s-multinode
```

### Configuração de mapeamento de porta (Port forwarding) para a máquina host

O kind suporta o mapeamento de porta para nós de controle e nós de trabalho. Isso permite que você acesse os serviços em execução no cluster a partir do host. Para configurar o mapeamento de porta, adicione a porta de mapeamento ao nó relevante:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    listenAddress: "0.0.0.0" # Opcional, o padrão é "0.0.0.0"
    protocol: udp # Opcional, o padrão é tcp


```

Implemente:

```yaml
# Um cluster com 3 control-plane nodes e 3 workers
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: control-plane
- role: control-plane
- role: worker
  # Mapeiar a porta 8080 do host para a porta 30000 do container
  extraPortMappings:
  # porta do container que eu quero expor
  - containerPort: 30000
    # porta da minha máquina local
    hostPort: 8080
    # vai escutar qualquer ip da minha máquina local
    listenAddress: "0.0.0.0" # Opcional, o padrão é "0.0.0.0"
 #   protocol:  tcp # Opcional, o padrão é tcp
- role: worker
- role: worker
```

### Ingress controller com kind

Em Kubernetes, o "Ingress Controller" é um componente responsável por gerenciar o tráfego de entrada (ingress) externo para os serviços dentro do cluster. Quando você cria um serviço em Kubernetes, ele pode ser acessado internamente pelos pods dentro do cluster, mas normalmente não é acessível externamente.

Para permitir o acesso externo a esses serviços, você precisa de um mecanismo que roteie as solicitações externas para os serviços corretos dentro do cluster. É aí que o "Ingress Controller" entra em ação.

O "Ingress Controller" é um controlador de recursos específico para lidar com objetos de Ingress no Kubernetes. O objeto Ingress é um recurso que define regras de roteamento para o tráfego de entrada externo com base no nome do host ou no caminho da URL. Ele atua como uma camada intermediária entre o tráfego externo e os serviços internos, direcionando as solicitações para os serviços apropriados com base nas regras definidas no recurso Ingress.

Existem vários Ingress Controllers disponíveis para Kubernetes, como o Nginx Ingress Controller, o Traefik Ingress Controller, o HAProxy Ingress Controller, entre outros. Cada um deles possui suas próprias características e funcionalidades específicas, mas todos têm o mesmo propósito básico de permitir o acesso externo aos serviços dentro do cluster.

O Kind (Kubernetes in Docker) é uma ferramenta que permite criar clusters Kubernetes usando containers Docker como nodes. O Kind é útil para criar ambientes de desenvolvimento e teste, bem como para experimentar com o Kubernetes em um único nó. O Kind não inclui um Ingress Controller por padrão, mas você pode implantar um Ingress Controller em um cluster Kind como faria em qualquer outro cluster Kubernetes.

Link para implementar o ingress controller com kind: [Ingress controller com kind ✔️](https://kind.sigs.k8s.io/docs/user/ingress/)


```yaml
 kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
```

Implemente:

```yaml
# a cluster with 3 control-plane nodes and 3 workers
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
- role: control-plane
- role: control-plane
- role: worker
- role: worker
- role: worker
```

**Instalando o ingress nginx com o comando kubectl**

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

## Vizualizando kubeconfigs

```shell
kubectl config view
```

## Alternando Clusters

```shell
kubectl config use-context <nome_cluster>
```
