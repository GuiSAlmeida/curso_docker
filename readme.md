# Docker

## Conceitos
### Imagem
Imagem é a **aplicação** que queremos rodar.

### Container
Container é a **instância da imagem** rodando como um **processo**.

### Volumes
Mecanismo para **persistir os dados** do container para fora deles.

## Comandos

### Flags úteis ao criar container (docker run)
`--name`  
Nomear container.  

`--detach | -d`  
Rodar instância em background.  

`--publish | -p <porta_local>:<porta_container>`  
Publicar uma porta.  

`-P`  
Mapeia porta com qualquer valor.  

`-v $(pwd):<path_container>`  
Para mapear volume, "$(pwd)" mapeia para pasta atual local.  

`--network <network_name>`  
Cria rede para comunicação entre containers.  

`-e <variavel>=<valor>`  
Cria variável de ambinete no container.

---
### Imagens
Baixar imagem:
```
docker pull <image>[:image_tag]
```

Criar instância de uma imagem (container):
```
docker container run <uuid | name>
```

Subir instância já criada:
```
docker container start <uuid | name>
```

Configurações da imagem:
```sh
docker image inspect <uuid | name>

# exemplo com format
docker image inspect --format '{{.Config.ExposedPorts}}' 49533628fb37
output: map[22/tcp:{}]
```

Salvar modificações na imagem (cria uma nova camada):
```
docker container commit -m "message" <uuid | name>
```

Ver camadas (alterações) de uma imagem:
```
docker image history <uuid | name>
```

Nomear e dar tag para imagem:
```
docker image tag <image_id> <image_name>[:image_tag]
```

Apagar imagem:
```
docker rmi <image>
```
---
### Containers
Lista todos containers:
```
docker container ls -a
```

Matar container:
```
docker container stop <uuid | name>
```

Apagar container:
```
docker container rm <uuid | name>
```

Apagar todos containers criados:
```
docker container prune
```

Lista processos do container:
```
docker container top <uuid | name>
```

Configurações do container:
```
docker container inspect <uuid | name>
```

Mostrar estatisticas:
```
docker container stats <uuid | name>
```

Acessar(anexar) um container:
```
docker container attach <uuid | name>
```

Rodar comando dentro de um container (sem precisar entrar no container):
```
docker container exec <uuid | name> <comando>
```

Verificar portas
```
docker port <uuid | name> [port]
```

### Volumes
Os volumes podem ser mapeados na criação da instância, passando o camando: `-v source:destination`.  
Source = local na máquina  
Destination = local no container  

Listar volumes:
```
docker volume ls
```

Inspecionar Volume:
```
docker volume inspect <uuid | name>
```

Configurando volume:
```
docker container run -v $(pwd):/workspace ...
```

### Network
Rede padrão que o docker cria quando os containers são iniciados.  
São divididas em `bridge`, `host`, `none`.  
Criar rede e remover:
```sh
docker network create <network_name>
docker network rm <network_name>
```

Listando redes docker:
```sh
docker network ls
```
Inspecionando redes:
```
docker network inspect <network>
```
Conectando e desconectando containers de uma rede:
```
docker network connect <network> <container>
docker network disconnect <network> <container>
```

### Dockerfile
https://docs.docker.com/engine/reference/builder/  
Criar imagem do zero.

`Dockerfile`
```docker
# Comment
INSTRUCTION arguments

# Example
RUN echo 'we are running some # of cool things'

# FROM - sets the base image.
FROM <image> [AS <name>]

# LABEL - adds metadata to an image,
# replace MAINTAINER(deprecated).
# Example: LABEL maintainer="Guisalmeida"
LABEL <key>=<value> <key>=<value> ...

# ADD - copies new files, directories or remote file URLs
ADD [--chown=<user>:<group>] <src>... <dest>

# COPY - copies new files or directories
COPY [--chown=<user>:<group>] <src>... <dest>

# RUN - execute any commands in a new layer.
RUN <command> | ["executable", "param1", "param2"]

# EXPOSE - listens on the specified network ports at runtime.
EXPOSE <port> [<port>/<protocol>]

# CMD - provide defaults for an executing container.
CMD <command> <params> | [["executable"], "param1", "param2"]

# USER - sets the user name.
USER <user>[:<group>] | <UID>[:<GID>]

# VOLUME - creates a mount point with the specified name and marks it as holding esternallymounted volumes from native host or other containers.
VOLUME ["/path"]
```

Comando que vai gerar imagem a partir do `Dockerfile`:
```bash
docker build [OPTIONS] <PATH | URL>

# options:
--tag | -t # add tag to an image
```

### Subir imagem docker hub
Logar via terminal
```
docker login
```

Subir imagem no [docker hub](https://hub.docker.com/)
```
docker push <repository>[:image_tag]  
```

### 4. Docker Compose
It is a tool for defining and running **multi-container** Docker applications.

**`docker-compose.yml`**  
```yml
version: '3'
services:
  web:
    ports:
    - '5000:5000'
  redis:
    image: redis
```

Commands to run compose:
```
docker-compose up

docker-compose up -d # roda em detach

docker-compose up --scale <service>=<number> # cria um quantidade definida do serviço especificado

docker-compose up --build # cria imagem e sobe conatiner que está especificada na flag build do `docker-compose.yml` que especificar onde está o `Dockerfile` para criação.

docker-compose -f file.yml up # para rodar arquivos com nome diferente

docker-compose down # finaliza e apaga containers criados

docker-compose -f file.yml down # finaliza e apaga containers com nome diferente

docker-compose stop # para container mas não apaga

docker-compose run # cria container rodando um comando especifico na sua criação

docker-compose build # cria imagem que está especificada na flag build do `docker-compose.yml` que especificar onde está o `Dockerfile` para criação.
```
#### 4.1. Depends_on
Quando um serviço depende de outro.

#### 4.2. Networks
Para adicionar rede ao qual o serviço vai pertencer.



