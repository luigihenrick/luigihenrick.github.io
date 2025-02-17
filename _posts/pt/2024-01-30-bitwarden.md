---
layout: post
title: Bitwarden (Gerenciador de senhas)! 🗄️
category: posts
lang: en
tags: [dev, host, local, lab]
image: /assets/img/posts/bitwarden-logo.jpg
accent_image: 
  background: url('/assets/img/posts/bitwarden.svg') center/20%
  overlay: true
description: >
  Tenha o total controle de suas senhas!
invert_sidebar: false
---

Algumas semanas atrás escrevi sobre o NAS e alternativas de mini servidores, sempre pensando em eficiência energética e a funcionalidade que busca alcançar, e dando continuidade ao assunto, gostaria de falar hoje sobre self-host e toda uma comunidade que descobri.

No artigo de umas semanas atrás, dei alguns exemplos de utilidades para um mini servidor, acaba que ele é muito mais útil do que imaginei de início, a minha ideia agora é falar um pouco mais sobre como está minha infraestrutura, quais apps self hosted tenho utilizado e me aprofundar nos que achar mais interessantes.

Dando início a essa série gostaria de falar mais sobre um dos apps que mais uso no dia-a-dia, o bitwarden.

O bitwarden é um cofre de senha, trabalhando em TI sempre soube dos riscos de utilizar a mesma senha em todos os serviços, e que só habilitar a autenticação em 2 fatores não garante sua segurança, foi daí que então comecei a pesquisar mais sobre cofres de senha, a princípio não queria pagar mensalidade ainda mais por ter que confiar armazenar todas minhas senhas em algum servidor de uma empresa terceira e que algumas da área já estiveram envolvidas em vazamentos de dados. A primeira alternativa que encontrei foi o KeepasXC, que é open source, conta com diversas opções de algoritmos de encriptação extremamente seguros e ainda possi app mobile, o único problema que encontrei é que ele armazena tudo em um arquivo no seu dispositivo, então você mesmo tem que fazer o backup ou utilizar um app como syncfy para sincronizar o arquivo entre diferentes dispositivos. 

Mas ainda não estava contente, não parecia ser a melhor alternativa, e então encontrei o bitwarden, a empresa que o mantém também oferece o serviço de hospedagem, caso prefira não ter o trabalho do hospedar você mesmo, e o código fonte do app é aberto, também conta com todas as vantagens do KeePassXC porém com interface web e uma imagem docker que pode facilmente ser utilizada para self-hosting.

Também conta com app mobile, autenticação por digital, importação de senhas de outros apps como Google chrome e há ainda algumas vantagens entre a versão self hosted e a versão gratuita utilizando os servers da empresa, no self hosting tem disponível autenticação totp, a mesma utilizada pelo Google e Microsoft authenticator e que gera códigos temporários para login.

Deixo aqui o comando docker para iniciar uma nova instância do server para caso queira testar:

```
docker pull vaultwarden/server:latest
docker run --detach --name vaultwarden \
  --env DOMAIN="https://vw.domain.tld" \
  --volume /vw-data/:/data/ \
  --restart unless-stopped \
  --publish 80:80 \
  vaultwarden/server:latest
```

Porém recomendo utilizar o docker compose, quando tiver mais apps em seu server e precisar gerenciar isso tudo, facilitará muito sua vida:

```
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: unless-stopped
    environment:
      DOMAIN: "https://vw.domain.tld"
    volumes:
      - ./vw-data/:/data/
    ports:
      - 80:80
```

E por esse post é isso, minha configuração inicial deixei disponível o server somente na rede local, porém para ter o meu cofre de senha disponível de qualquer lugar configurei um tunnel da cloudflare que abordarei como configurar no post a seguir.