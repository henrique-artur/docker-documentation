# Guia Rápido: Docker

## Instalação

### 1 - Atualize os pacotes do linux

```
    $ sudo apt-get update
```

### 2 - Instale as dependencias necessárias para a instalação do docker

```
    $ sudo apt-get install ca-certificates curl gnupg
```

### 3 - Adicione a chave GPG oficial do Docker

```
    $ sudo install -m 0755 -d /etc/apt/keyrings
```

```
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

```
    $ sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### 4 - Defina o repositório Docker

``` 
    $ echo \
        "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
        "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
        sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
 ```

### 5 - Atualize novamente os pacotes do sistema

``` 
    $ sudo apt-get update
```

### 6 - Por fim instale o docker

```
    $ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 7 - Verifique se o Docker está funcionando corretamente executando o comando

```
    $ sudo docker run hello-world
```

> Se tudo ocorrer bem seguindo a configuração acima, você verá uma mensagem como essa em seu terminal:
![imagem](https://user-images.githubusercontent.com/61638364/236953514-942e0366-f031-4b8c-bb8b-3ba5ffc60428.png)

# Concedendo privilégios ao docker

> Conceder privilégios ao docker evita possiveis problemas e tira a necessidade de usar sudo o tempo todo.

```
    $ sudo groupadd docker
```
```
    $ sudo usermod -aG docker $USER
```

OBS.: Se não houve nenhuma mensagem de erro, indica que funcionou.

# Containerizando uma aplicação

### 1 - Clone um projeto 

```
    $ git clone https://github.com/docker/getting-started.git
```

### 2 - Entre na pasta `app` dentro da pasta main do projeto

```
    $ cd getting-started/app
```

### 3 - Crie um arquivo `Dockerfile` e adicione o código abaixo:

```
    FROM node:18-alpine
    WORKDIR /app
    COPY . .
    RUN yarn install --production
    CMD ["node", "src/index.js"]
    EXPOSE 3000
```

### 4 - Agora construa a imagem com o seguinte comando:

```
    $ docker build -t getting-started .
```

### 5 - Pro fim, execute: 

```
    $ docker run -dp 3000:3000 getting-started
```

> Se tudo ocorrer bem ele ira retornar o ID do container e sua aplicação estará em execução no seu localhost:3000, assim como nas imagens abaixo:
![imagem](https://user-images.githubusercontent.com/61638364/236956906-84fe88f0-0b50-4d24-8b34-2c8c44bd9242.png)
![imagem](https://user-images.githubusercontent.com/61638364/236957167-0e3fdcd3-d2d9-46c5-87bd-8fd35a7a9193.png)

# Versionamento

### 1 - De início logue em seu docker hub

```
    $ docker login
```

### 2 - Faça suas mudanças na sua aplicação
> Lembre-se de criar o repositorio no docker hub.

### 3 - Publique sua imagem

```
    $ docker push henrique-artur/getting-started
```

### 4 - E finalmente, commite suas mudanças

```
    $ docker commit <container-id> henrique-artur/getting-started
```

