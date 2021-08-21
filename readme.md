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
docker pull <image>:[version]
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



