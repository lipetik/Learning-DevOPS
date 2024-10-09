# Docker Basics 

## Dockerfile

### O que e um dockerfile ?
O bloco fundamental para criar uma imagem docker e um arquivo docker file. Este arquivo contem instruções para a criação de imagens . 

- Abaixo está um exemplo de docker file .
~~~
FROM debian:latest
LABEL maintainer="Macoratti"
RUN apt-get update && apt-get upgrade -y
RUN apt-get install nginx -y
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
~~~

### Como funciona uma dockerfile ? 

Uma docker image é organizada em camadas. De forma que somente a ultime e read and write e todas abaixo delas apenas read . Toda imagem docker é criada de uma imagem base ou imagem parente. 

### Estrutura de uma dockerfile:

Temos fundamentalmente duas partes: 
1. **instrução**: FROM, RUN, WORKDIR, COPY, etc.. 
2. **Argumento**: é o argumento que a instrução recebe. 

Exemplo: O comando RUN roda o argumento no terminal e atualiza o container .  

~~~
RUN apt-get update && apt-get upgrade -y
~~~

### Lista com algumas instruções importantes para roda uma dockerfile: 


- **FROM**: especifica a imagem base que será utilizada.
- **RUN**: executa comandos durante o *build* da imagem.
- **ENV**: seta variáveis de ambiente dentro da imagem, este comando está disponível tanto na build como dentro do container em funcionamento.
- **COPY**: copia arquivos locais para diretórios dentro da imagem.
- **EXPOSE**: especifica a porta que sera exposta do container docker.
- **ADD**: é uma espécie de comando COPY com mais funções. É recomendado a preferência ao uso do comando COPY. 
- **WORKDIR**: Seta o ambiente de trabalho atual. Ao setar o WORKDIR os outros comando como RUN, CMD, ADD, COPY ou ENTRYPOINT serão executados nesse diretório .
- **VOLUME**: E o comando usado pra criar ou montar um volume para o container docker.
- **USER**: Seta o nome de usuário e UID quando rodando em um container. Pode usar essa instrução para colocar um non-root user no container .
- **LABEL**: É usado para especificar metadata da imagem docker .    
- **ARG**: Usado para setar variáveis build-time com chave e valor. O comando ARG não estará mais disponível quando o container estiver rodando.
- **CMD**: É usado para executar um comando num container que já está funcionando. Obseve caso sejam utilizado vários comando CMD apenas o último funcionará. Este comando pode ser overridden do container CLI. 
- **ENTRYPOINT**: Parecido com o comando CMD. Em caso de dúvida leia CMD x ENTRYPOINT no seguitne link:  https://devopscube.com/run-scripts-docker-arguments/.


## Criando uma imagem docker com Dockerfile

Nesse exemplo vamos criar uma imagem docker do nginx com uma página web custom. Para isso vamos fazer os seguintes passos: 

1. Vamos cria as pastas e arquivos 

- Criando a pasta nginx-image
~~~
mkdir nginx-image 
~~~

- Entrando na pasta criada
~~~
cd nginx-image
~~~

- Criando um arquivo .dockerignore
~~~
touch .dockerignore
~~~

2. Vamos agrora criar os arquivos de configuração e páginas do nosso nginx.

- Basta utilizar o vi/vim 

~~~
vim index.html
~~~

- Neste arquivo copie e salve o seguinte codigo:

~~~
<html>
  <head>
    <title>Dockerfile</title>
  </head>
  <body>
    <div class="container">
      <h1>Me Primeiro  App</h1>
      <h2> Este e o meu primeiro app</h2>
      <p>Hello everyone, This is running via Docker container</p>
    </div>
  </body>
</html>
~~~

- Faça o  mesmo para o arquivo de configuração default


~~~
vim default
~~~


~~~
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    
    root /usr/share/nginx/html;
    index index.html index.htm;
    server_name _;
    location / {
        try_files $uri $uri/ =404;
    }
}
~~~


3. Saia da pasta pasta com o comando 

~~~
cd .. 
~~~

4. Cria a Dockerfile: 

- Abra a Dockerfile utilizando seu editor de texto de preferência
~~~
vim Dockerfile
~~~

- Edite sua dockerfile
~~~
# Base image
FROM ubuntu:latest

# Use LABEL for metadata as recommended
LABEL maintainer="contact@devopscube.com"

# Install nginx
RUN apt-get update -y && apt-get install -y nginx

# Copy configuration files and content
COPY files/default /etc/nginx/sites-available/default
COPY files/index.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80

# Start nginx
CMD ["/usr/sbin/nginx", "-g", "daemon off;"]
~~~

5. Cheque se a sua estrutura está de arquivos e pastas está correta. Ela deve ter a seguinte forma: 


~~~
nginx-image
├── Dockerfile
└── files
    ├── default
    └── index.html
~~~

6. Agora estamos preparado para fazer a build da imagem. a

~~~
docker build -t nginx:1.0 .
~~~

- Observe que aqui o comando -t é para fazer o tagging da imagem. 
- 1.0 é a tag da imagem. 
- O . no final, significa que estamos fazendo referência a Dockerfile para o build.
- Observe que estamos onde esta localizado a nossa docker file. Caso contrário precisaríamos colocar o path. Como abaixo. 

~~~
sudo docker build -t nginx /path/to/folder
~~~

7. Agora podemos listar as imagens disponíveis: 

~~~
sudo docker image ls -a 
~~~

8. Agora podemos rodar o nosso container 
~~~
sudo docker run -d -p 9090:80 --name webserver nginx:1.0
~~~

- -d é para rodar o container em *detached* mode 
- -p 9090:80 é a flag para porta, no formato: localport:container-port
- --name é a flag para designar o nome do container. Nesse caso, webserver.

9. Podemos listar os containers na nossa máquina:

~~~
sudo docker container ls
~~~

- Podemos utilizar também o

~~~
sudo docker ps
~~~

10. Agora no seu browser basta ir até o link http://<host-ip>:9090 e verificar se a página é a página custom criada anteriormente. 




