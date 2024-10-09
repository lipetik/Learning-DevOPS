# Instalação e configuração do Docker 


## Instalação no Linux

### Metodo 1: Installing the latest Docker Release [One Liner]

~~~ 
curl -sSL https://get.docker.com/ | sudo sh
~~~

### Method 2: Install Docker From the apt Repository

1. Faça a atualização do cache

~~~ 
sudo apt update -y
~~~

2. Adicione a chave GPG 
~~~
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
~~~

3. Adicione  o apt repositório do docker 
~~~
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
~~~

4. Atualize o cache novamente
~~~
sudo apt-get update -y
~~~

5. Instale todos os componentes do Docker;
~~~
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
~~~
6. Verifique a instalação do docker

- Verificando a versão do docker instalado
~~~
sudo docker version
~~~

- Testando se o serviço do docker está funcionando
~~~
sudo service docker status
~~~

- Caso o serviço não esteja funcionando user o service para iniciar o docker
~~~
sudo service docker start
~~~

- Caso o docker daemon não esteja funcionado, use o dockerd para inicia-lo e verificar possíveis problemas
~~~
sudo dockerd
~~~

## Validação da Instalação do Docker
Para validar a instalação do Docker vamos fazer algumas operações básicas do docker. 

### Download de imagem via docker
1. Vamos dar um pull em uma imagem básica do docker

~~~
sudo docker pull ubuntu:latest
~~~

2. Vamos verificar a imagem cujo o download foi feito
~~~
sudo docker image ls
~~~

### Vamos rodar um container com a imagem que foi baixada

1. Vamos rodar a imagem que foi baixada com um comando para iteratividade

~~~
sudo docker run -it --name teste ubuntu:latest
~~~

Observe que aqui o comando 
*  -it é importante para permitir a iteratidade do container via shell . 
*  --name apenas batiza o container criado com o nome que desejarmos, nesse caso teste. 

Sendo assim o comando criará um container rodando Ubuntu com uma bash shell que permite a interação do usuário com o container. Portanto estaremos dentro do container com uma bash shell funcional. 

2. Para sair do container bastar executar o comando:
~~~
exit
~~~

3. Depois de sair do container podemos listar todos os containers disponíveis utilizando qualquer um dos dois comando abaixo: 


~~~
sudo docker container ls -a 
~~~

~~~
sudo docker container ps -a
~~~

### Como rodar comandos docker sem utilizar o Sudo ? 

Por default docker precisa de autorização sudo para rodar. Para faze-lo funcionar sem ter que digitar sudo antes de qualquer comando docker é preciso fazer alguns passos. 

1. Precisamos adicionar o usuário ao Docker group

~~~
sudo usermod -aG docker $USER
~~~

2. Saia e entre novamente na sessão de terminal para que as mudanças tenham efeito.

3. Alternativamente para que as mudanças tenham efeito use o comando:
~~~
newgrp docker
~~~



