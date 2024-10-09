# Conceitos fundamentais de Container

## Introdução

### O que é Docker ? 
Docker é um container ! Esta resposta obviamente não ajuda muito já que não sabemos o que é um container . Portanto a pergunta a ser feita deve ser: O que é um container ? 

### O que é um container ? 

É um conjunto de recursos agrupados de uma aplicação junto com suas dependências que compartilham o mesmo kernel do sistema operacional hospedeiro (host) . Container significa isolar na máquina hospedeira um parte que utiliza o kernel do host para funcinar sem grande overhead se comparado a aplicação rodando nativamente no sistema operacional. 

Isto significa que na mesma máquina é possível rodar diversas aplicações isoladas utilizando containers sem grande aumento de custo computacional devido ao compartilhamento do kernel da máquina hospedeira. 

## VM x Containers ( Qual a vantagem de utilizar containers ? )

Enquanto Containers utilizam o kernel compartilhado do host, VMs são virtualizações de outros sistemas operacionais. Isto significa que cada máquina precisa de seu próprio Kernel para emular o sistema operacional, sendo assim menos eficientes do ponto de vista de economia de recursos computacinais . 


## Benefícios de containers : 

- Overhead reduzido: containers são mais leves, e portanto consomem menso recursos;
- Startup rápido: containers podem ser iniciados quase que instantaneamente, quando comparados a máquinas virtuais; 
- Melhor uso dos recursos computacionais: Compartilhar o kernel do host permite um uso mais eficiente dos recursos do sistema. 


## User Space e Kernel Space

### O que é Userspace ? 

Userspace são os espaços acessíveis ao usuário . Isto quer dizer que aplicações como WebServers, Chrome, Editores de texto não tem acesso direto ao kernel ou a recursos de hardware. Aplicações no userspace precisam fazer um requisição ao kernel para determinado serviço .  

Por exemplo: se um userspace quiser criar um arquivo, ele precisa realizar uma system call para que o kernel e o kernel então realizará esta ação. 

### O que é Kernelspace ? 

Este é o espaço onde o núcleo do sistema operacional funciona. Ou seja, onde o kernel opera. O kernel possui acesso irrestrito ao hardware (memoria, processamento, etc ...) . O kernel recebe as system calls e opera as atividades necessárias para traduzir as instruções do userspace para o hardware do sistema. 


- Exemplo: O comando ls (list files):

 1. O comando ls é executado no userspace (terminal);
 2. O comando ls então faz uma system call para o kernel para listar os arquivos no diretório atual;
 3. O kernel então ao receber essa system call faz a operação requisitada;
 4. O kernel então retorna os resultados da operação para o comando ls;
 5. Finalmente o comando ls mostra os resultados para o usuário.  

## Isolamento de processos 

Ja falamos anteriormente que **a principal função de um container é isolar aplicações**. Podemos entender isso da seguinte forma: uma aplicação como Nginx ao ser iniciada é nada mais que um processo na máquina e um processo por sua vez nada mais é que um conjunto de intruções que funcionam de forma isolada . 

A ideia de um container e fazer com que esse isolamento seja ainda mais forte . Em vez de utilizar o mesmo ambiente do host pra rodar a nossa aplicação utilizamos um container. Ou seja, um ambiente criado e configurado apenas com os aquivos necessários para fazer com que a aplicação funcione.

Ao isolar mais fortemente as aplicações dentro de um container promovemos sua estabilidade, segurança e portabilidade . Ou seja, a nossa aplicação rodará em vários outros sistemas sem problemas.

Perceba também que cada container tem seu userspace único. Isto permite que no mesmo host seja possível ter funcionando diferentes distribuições Linux, todas dividindo o mesmo kernel. 

## Gerenciando Multiplos Processos dentro de um container 

Se um container é uma coleção de processos, obviamente deve ser capaz de lidar com mais de um processo certo ? Na verdade, não. Cada container consegue ter apenas um main process, que é determinado ao iniciar o container . Esse processo principal (main process) é o responsável por gerenciar qualquer atividade adicional dentro do container que ele precise para funcinar. 

### Porque o container permite apenas um parent process ? 

**Containers foram projetados para rodar apenas um processo por container !**


Esta escolha de desing foi feita para promover isolamento  e ajuda a manter o container estável . Ou seja, caso ocorra algum problema naquele processo, este processo impacta somente o container cujo aquele processo roda, mantendo os outros funcionando sem problemas isolados. 

- Exemplo: imagine que queiramos rodar uma aplicação web. E nessa aplicação precisamos de um server web e um database . Em vez de rodarmos ambos em um único container . Podemos ter o servidor web rodando em um container e este se comunicar com o banco que está rodando em outro container . Em caso de vulnerabilidade no código do server, o banco de dados continuára funcionando sem problemas porque estará isolado em seu próprio container . 


### É possível rodar vários processos em um container ? 

Podemos rodar vários processos dentro de um container  usando systemmd ou supervisord. Mas funciona um pouco diferente, o process manager utilizado serve como o processo principal do container  e controla vários child processes . Dessa forma podemos contornar a limitação de um único processo . Mas lembre-se, o container continua rodando um único processo . O que ocorre e que o process manager controla os outros processos no container. 

## Conceitos fundamentais do Linux que permitem containers 

O isolamento de containers é possível por causa de duas *features* do linux: 
 1. Namespaces 
 2. Control groups (Cgroups)  

Podemos pensar nesses conceitos como um Prédio. Cada prédio tem vários apartamentos. Cada apartamento é separado e privado com seu próprio número e dependências utilitárias. No caso de containers, cada um opera isolado com seus própio valor de CPU, memory, IP address, mount points, and processes. Isto é feito através de namespaces e control groups do kernel do Linux . 

## Linux Namespaces
1. PID Namespace: Isola o ID dos processos;
2. Network Namespace(NET): controla a interface de network;
3. IPC Namespace (IPC): controla a interface de recursos *interprocess communication (IPC)*; 
4. Mount Namespace (MNT): controla os pontos de montagem do *filesystem* ;
5. UTS Namespace (UTS): esse namespace isola o kernel e identificadores de versões;
6. User Namespace (USR): o namespace separa o user IDs do host e do container.
7. Control Group Namespace(cgroup): controla as informações do control group da informações dos procesos de containers . 

- É possível listar todos os namespaces em uma máquina linux usando o comando: 
~~~
lsns
~~~


## Linux Control Groups 
Quando o linux começa um processo não é necessário alocar manualmente limites de memória e CPU. O kernel faz este trabalho automaticamente e prioriza alocação de recursos em serviços que estão rodando. Mas, caso seja necessário, é possível alocar manualmente um limite de memória e CPU utilizando uma **feature** do Kernel do Linux chamado de Control Group(CGroup) . 


Observe que podemos pensar em cada container como uma máquina em separado rodando no nosso host . Isto quer dizer que o limitar o consumo de CPU/Memória de cada container é particurlamente útil nesse caso. E se torna possível por causa do CGroups. Portanto CGroups limita e controla os recursos que cada container pode utilizar. Observe que caso esse controle não fosse feito, é possível que um único container consumisse toda capacidade da máquina host deixando as outras com *fome* de recursos. 

!


