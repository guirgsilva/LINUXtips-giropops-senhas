# Giropops Senhas - Documentação

Este projeto consiste em uma aplicação Flask chamada "giropops-senhas" que se conecta a um serviço Redis. A aplicação e o Redis são executados em contêineres Docker separados.

## Passos para criar e executar a aplicação

1. **Criação do Dockerfile:**

   O Dockerfile contém instruções para criar uma imagem Docker para a aplicação Flask. O conteúdo do Dockerfile é o seguinte:

Use uma imagem oficial do Python como imagem pai
FROM python:3.11

Defina o diretório de trabalho no contêiner
WORKDIR /app

Adicione os arquivos do aplicativo ao contêiner
ADD . /app

Instale as dependências do aplicativo
RUN pip install --no-cache-dir -r requirements.txt

Inicie o servidor HTTP do Prometheus em um processo separado
RUN echo 'from prometheus_client import start_http_server; start_http_server(8089)' > prometheus_server.py

Faça o contêiner ouvir na porta 8088 em tempo de execução
EXPOSE 8088

Inicie o aplicativo Flask e o servidor HTTP do Prometheus em paralelo
CMD ["sh", "-c", "python prometheus_server.py & python app.py"]


2. **Construção da imagem Docker:**

Com o Dockerfile criado, construa a imagem Docker usando o comando:

docker build -t giropops-senhas .


3. **Execução do contêiner da aplicação:**

Inicie um contêiner a partir da imagem `giropops-senhas` usando o comando:

docker run -d --name giropops-senhas-container -p 8088:8088 giropops-senhas


4. **Criação do contêiner Redis:**

A aplicação Flask precisa se conectar a um serviço Redis. Para isso, puxe a imagem oficial do Redis do Docker Hub e inicie um contêiner Redis:

docker pull redis docker run -d --name redis-service -p 6379:6379 redis


5. **Configuração da conexão entre a aplicação e o Redis:**

Para permitir que a aplicação Flask se conecte ao Redis, crie uma rede Docker e adicione ambos os contêineres a essa rede:

docker network create app-network 
docker network connect app-network redis-service 
docker network connect app-network giropops-senhas-container


6. **Verificação do tamanho da imagem Docker:**

Para verificar o tamanho da imagem Docker criada, use o comando `docker images`.

7. **Limpeza de imagens Docker não utilizadas:**

Para manter o sistema limpo, use os comandos `docker image prune` e `docker rmi` para remover imagens Docker não utilizadas.


