# Docker Basics

## Introdução

### O que é Docker ? 

*Docker is “an open-source project to pack, ship and run any application as a lightweight container.”* 

Em tradução livre seria algo como : Um projeto open source feito para projetar, enviar e rodar qualquer aplicação tão leve quanto um container . 

A ideia e que docker seria um sistema para manusear containers mas focados em aplicações. E containers em si são muito úteis por eles podemos mandar mercadorias para todo mundo de forma padronizada . Containers foram uma revolução na logística nesse sentido. Docker são nada mais que containers feitos para transportar aplicações. 

### Porque Docker ?

1. Manter as aplicações isoladas e independentes para que não interfiram uma com a outra. 

2. Facilidade de *deploy* : fazer com que mover a aplicação de desenvolvimento para produção seja muito mais rápido. 

3. Manter a consistência, ou seja, garantir que a aplicação rode da mesma maneira em qualquer lugar.                    

## Docker Core Architeture

O docker tem três principais componentes: 

1. Docker Engine (dockerd)
2. docker-containerd (containerd)
3. docker-runc (runc)


### Docker Engine

A docker engine contem dentro de si três componentes importantes: 

1. Docker daemon (dockerd) : Este componente roda continuamente 
2. API interface
3. Docker CLI 



