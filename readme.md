# Kind Install ☸ 👨‍💻 

**Link da documentação do kind Kubernetes**

Clique aqui - [Kind ✔️](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)




🔵 **OBS!** O kind e baseado no kubeadmin

🔵 **OBS!** Antes de criar um cluster com o kind certique-se de que o kubectl esteja instalado, caso não esteja, clique aqui [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

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
