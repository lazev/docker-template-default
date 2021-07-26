![](https://img.shields.io/github/issues/marcelocostabr/docker-template-default)
![](https://img.shields.io/github/forks/marcelocostabr/docker-template-default)
![](https://img.shields.io/github/stars/marcelocostabr/docker-template-default)
![](https://img.shields.io/github/license/marcelocostabr/docker-template-default)

# Modelo de Docker Server
[Nginx + PHP 8.0 + MariaDB 10.5 + PHPMyAdmin]

Esta configuração contém:

| Serviço    | Versão       |
|------------|--------------|
| Nginx      | Última       |
| PHP        | FPM:8.0      |
| Database   | MariaDB:10.5 |
| PHPMyAdmin | Última       |

As configurações do docker estão dentro da pasta .docker:

| Pasta         | Descrição          |
|---------------|--------------------|
| .docker/db    | Arquivos do banco de dados (será criada na instalação) |
| .docker/nginx | Configuração do servidor Nginx |
| .docker/php   | Arquivo Dockerfile para o PHP |
| .docker/ssl   | Arquivos de certificado para usar o HTTPS |

## Para usar, siga estes passos:


## 1) Clone esse repositório

Na pasta raís do seu projeto digite

```
git clone https://github.com/lazev/docker-template-default
```

## 2) Certificado para HTTPS

Para acesso por HTTPS é preciso ter um certificado (pode ser um auto-assinado para ambiente de desenvolvimento).

No linux o comando é:

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout cert.key -out cert.crt
```

Nas perguntas que aparecem, a linha mais importante é a
**Common Name (e.g. server FQDN or YOUR name) []:**
onde você precisa informar o nome do seu servidor
(o endereço que será usado para acessar pelo navedor).

O padrão aqui é **dev.local**, se preferir outro, também é necessário alterar nas configurações do sevidor, mais abaixo.

Mova ou copie ambos arquivos cert.key e cert.crt para a pasta .docker/ssl.

Para não usar HTTPS, comente as seguintes linhas no arquivo de configuração do Nginx (.docker/nginx/default.conf):

```
listen 443 ssl;
ssl_certificate /etc/nginx/ssl/cert.crt;
ssl_certificate_key /etc/nginx/ssl/cert.key;
```

## 3) Configuração do servidor

**.docker/nginx/default.conf:**
Por padrão, a URL do projeto é **dev.local**, mas é possível alterar dentro das configurações do Nginx, na linha

```
server_name dev.local www.dev.local;
```

**/etc/hosts:**
O mesmo nome informado na configuração do Nginx precisa estar no hosts da máquina.

No linux, rode o seguinte comando como ROOT:

```
echo "127.0.0.1  dev.local www.dev.local" >> /etc/hosts
```

Caso tenha escolhido outro nome na configuração do Nginx, é preciso trocar aqui também.


## Comandos básicos do docker

**Iniciar o servidor**
A opção -d não prende a tela do terminal, omita esta opção se deseja visualizar as informações do servidor.

```
docker-compose up -d
```

**Parar o servidor**
Se a tela do terminal estiver presa com as informações do servidor, basta apertar CTRL+C para pará-lo.

```
docker-compose down
```

**Refazer o servidor**

```
docker-compose up -d --build
```

**Listar todos os containers ativos**

```
docker ps -a
```

**Parar e remover todos os containers**

```
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)
```


## Navegador

Para acessar site, os caminhos comuns são

**http**
[http://localhost](http://localhost) or [http://dev.local](http://dev.local)

**https**
[https://localhost](https://localhost) or [https://dev.local](https://dev.local)
